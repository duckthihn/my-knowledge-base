---
type: problem
title: AWS Glue Job Optimization and Cost Reduction
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-glue]]", "[[learn-spark]]", "[[apache-parquet]]"]
tags: [aws, glue, spark, optimization, cost-reduction, dpu, troubleshooting, skew, spill]
status: stable
---

# AWS Glue Job Optimization and Cost Reduction

In AWS Glue's serverless billing model, technical pipeline efficiency directly dictates cloud financial expenditure (see [[aws-glue]]). Because pricing is calculated as $\text{Cost} = \text{DPUs} \times \text{Runtime (hours)}$, performance bottlenecks like memory spills, data skew, un-coalesced small files, and un-optimized worker sizing directly inflate infrastructure bills (see [[aws-glue]], [[aws-glue-architecture-and-dpu]]).

## The 6 Critical Performance & Cost Anti-Patterns

### 1. Scaling Worker Count to Address Data Skew
- **The Mistake**: Adding more workers (e.g., increasing from 10 to 40 G.1X workers) when a job stalls at 99% during a `groupBy` or `join` (see [[aws-glue]], [[debugging-slow-spark-jobs]]).
- **The Failure**: Data skew routes all records for the dominant key to exactly one partition task running on a single executor core (see [[learn-spark]], [[debugging-slow-spark-jobs]]). The remaining 39 workers finish in seconds and sit completely idle while the single straggler task runs for an hour, multiplying DPU-hour costs with zero speedup (see [[aws-glue]]).
- **Actionable Remedy**: Identify the skewed key in the Spark UI, apply salting to redistribute records across hash buckets, or enable Adaptive Query Execution skew join handling (`spark.sql.adaptive.skewJoin.enabled = true`) (see [[debugging-slow-spark-jobs]]).

### 2. Under-Sizing Worker Types Leading to Memory Spill
- **The Mistake**: Running memory-heavy aggregations or large window operations on basic G.1X workers (16 GB RAM per worker) (see [[aws-glue]]).
- **The Failure**: When per-task data exceeds execution memory, Spark spills data to local executor disks (`Spill (Memory)` and `Spill (Disk)`), multiplying stage duration by 5x–10x due to serialization and disk I/O (see [[learn-spark]], [[debugging-slow-spark-jobs]]).
- **Actionable Remedy**: Upgrade to **G.2X (32 GB RAM)** or **G.4X (64 GB RAM)** workers to provide adequate in-memory execution headroom, or increase shuffle partition counts to reduce individual partition sizes (see [[aws-glue-architecture-and-dpu]], [[spark-partitioning-and-shuffle]]).

### 3. Ingestion Bottlenecks and Small File Generation
- **The Mistake**: Reading un-compacted S3 datasets without grouping, and writing partitioned outputs with hundreds of shuffle partitions (see [[aws-glue]], [[parquet-small-files-problem]]).
- **The Failure**: Reading 50,000 tiny files spawns 50,000 tasks, overwhelming the driver with task scheduling overhead. At output, 200 default shuffle partitions generate 200 tiny files per date folder, perpetuating the small files problem (see [[aws-glue]], [[parquet-small-files-problem]]).
- **Actionable Remedy**:
  - On Read: Use `additional_options={"groupFiles": "inPartition", "groupSize": "134217728"}` to pack input files into 128 MB chunks (see [[glue-data-catalog]]).
  - On Write: Call `df.coalesce(optimal_partitions)` or configure AQE partition coalescing before invoking `.write.parquet()` (see [[spark-partitioning-and-shuffle]], [[parquet-small-files-problem]]).

### 4. Broken Job Bookmarks Causing Full Re-Scans
- **The Mistake**: Failing to provide a `transformation_ctx` on reader/writer nodes, or omitting `job.commit()` at the end of the script (see [[aws-glue]]).
- **The Failure**: Without all three components (job bookmark enabled, distinct `transformation_ctx`, and `job.commit()`), Glue cannot record state and re-scans the entire historical dataset on every scheduled run, causing runtime and cost to grow linearly over time (see [[aws-glue]]).
- **Actionable Remedy**: Always initialize the script with `job.init()` and seal the execution with `job.commit()`, assigning unique context strings to every source and sink:
  ```python
  dyf = glueContext.create_dynamic_frame.from_catalog(
      ..., transformation_ctx="source_orders"
  )
  # ... transformations ...
  job.commit()
  ```

### 5. Pipeline Over-Splitting in Workflows
- **The Mistake**: Decomposing an ETL pipeline into 4 or 5 separate Glue jobs (e.g., Job 1: Filter, Job 2: Select, Job 3: Transform) managed via a Glue Workflow (see [[aws-glue]]).
- **The Failure**: Every individual Glue job is a distinct Spark application paying a 2- to 4-minute cluster startup penalty and writing intermediate data out to S3, only for the next job to read it back in (see [[aws-glue]], [[spark-execution-model]]).
- **Actionable Remedy**: Consolidate consecutive narrow transformations (`filter`, `select`, `withColumn`) into a **single Glue job**. Pipeline narrow operations in-memory and only split jobs when distinct compute profiles (e.g., G.1X vs. G.4X) or hard data quality checkpoints are required (see [[aws-glue]], [[spark-execution-model]]).

### 6. Missing Job Timeout Protection
- **The Mistake**: Leaving the Glue job timeout at its default setting of **48 hours (2,880 minutes)** (see [[aws-glue]]).
- **The Failure**: If a script enters an infinite retry loop, deadlocks on a JDBC connection, or stalls on skew, it can run continuously for 2 full days, generating thousands of dollars in unintended DPU charges (see [[aws-glue]]).
- **Actionable Remedy**: Enforce an explicit timeout on every job definition (e.g., setting `Timeout: 60` for a job expected to complete in 15 minutes) (see [[aws-glue]]).

## Sizing & Cost Optimization Checklist

Apply this operational checklist to audit and minimize Glue infrastructure costs (see [[aws-glue]], [[aws-glue-architecture-and-dpu]]):

| Optimization Lever | Implementation Strategy | Expected Benefit |
| :--- | :--- | :--- |
| **Enable Flex Execution** | Set `ExecutionClass: FLEX` for non-urgent batch, overnight runs, and backfills (see [[aws-glue-architecture-and-dpu]]). | 30%–50% cost discount per DPU-hour (see [[aws-glue]]). |
| **Right-Size Auto-Scaling** | Inspect CloudWatch `glue.driver.aggregate.numActiveExecutors` metric. Lower max worker limits if peak usage never reaches the ceiling (see [[aws-glue]]). | Eliminates over-allocation billing waste (see [[aws-glue-architecture-and-dpu]]). |
| **Upgrade Glue Version** | Migrate legacy jobs to Glue 4.0 or 5.0+ to leverage modern Spark engines and reduced DPU pricing tiers (see [[aws-glue]]). | Faster runtime and lower base hourly rates (see [[aws-glue]]). |
| **Apply `push_down_predicate`** | Filter partition keys at the Catalog read stage (`push_down_predicate="year == '2026'"`) (see [[glue-data-catalog]]). | Skips S3 directory listings and file scans entirely (see [[query-io-optimizations]]). |
| **Convert DynamicFrame to DataFrame** | Use DynamicFrame only for dirty data ingestion; convert via `.toDF()` for joins/aggregations (see [[dynamicframe-vs-dataframe]]). | Unlocks Spark Catalyst optimizations and reduces CPU time (see [[dynamicframe-vs-dataframe]]). |

## Related

- [[aws-glue]]
- [[aws-glue-platform]]
- [[aws-glue-architecture-and-dpu]]
- [[dynamicframe-vs-dataframe]]
- [[glue-data-catalog]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
- [[parquet-small-files-problem]]
- [[query-io-optimizations]]
