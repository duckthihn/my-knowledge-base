---
type: problem
title: Debugging Slow PySpark Jobs
created: 2026-11-13
updated: 2026-11-13
sources: ["[[learn-spark]]", "[[apache-parquet]]", "[[apache-iceberg]]", "[[aws-glue]]"]
tags: [spark, pyspark, troubleshooting, debugging, performance, optimization, skew, spill]
status: stable
---

# Debugging Slow PySpark Jobs

Diagnosing sluggish PySpark jobs requires shifting from vague assertions ("Spark is slow") to precise, hypothesis-driven isolation of execution plan bottlenecks, data skew, memory spills, and un-optimized shuffle exchanges (see [[learn-spark]]). Most severe performance degradation traces directly to wide transformation boundaries and improper partition sizing (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).

## The 3 Key Symptoms of Shuffle Inefficiencies

When a PySpark job stalls or performs poorly, look for these three primary indicators in the Spark UI (see [[learn-spark]]):

1. **Post-Transformation Stage Freezes**:
   - Stages exhibit excessive runtimes immediately following a `join`, `groupBy`, `distinct`, or `orderBy` (see [[learn-spark]]).
   - Indicates heavy network transfer, serialization delays, or inefficient join strategies (see [[spark-partitioning-and-shuffle]]).
2. **Task Spills (Memory → Disk)**:
   - Spark UI task metrics display `Spill (Memory)` and `Spill (Disk)` values (see [[learn-spark]]).
   - Occurs when input or shuffle partitions are too large, exhausting executor execution memory and forcing Spark to buffer records onto local disk, drastically increasing I/O latency (see [[learn-spark]]).
3. **Severe Data Skew (The "99% Task Trap")**:
   - 99 out of 100 tasks complete in seconds, but 1 or 2 straggler tasks take 50x longer or fail entirely (see [[learn-spark]]).
   - Happens when a particular key dominates the dataset (e.g., a single country code or null value represents 80% of rows), forcing a single executor to process nearly all data while others sit idle (see [[learn-spark]]).

## The 5-Step Systematic Troubleshooting Checklist

To isolate and resolve performance regressions systematically, execute this diagnostic workflow in sequence (see [[learn-spark]]):

### Step 1: Identify the First Expensive Stage
- Open the Spark UI **Stages** tab and sort stages by total execution duration (see [[learn-spark]]).
- Pinpoint what operation directly precedes the slow stage: is it a `join`, `groupBy`, `distinct`, or `orderBy`?
- *Rule*: Unless proven otherwise, assume wide transformation shuffle overhead is the bottleneck (see [[learn-spark]], [[spark-execution-model]]).

### Step 2: Check for Skew
- Inspect task timeline summaries and task metrics for min, median, 75th percentile, and max duration and input size (see [[learn-spark]]).
- If max duration and input size diverge significantly from the median, the stage suffers from data skew.
- *Remedy*: Simply adding more executors will **not** resolve skew because all skewed records for a key route to one executor. Instead, apply salting, filter null keys prior to joining, or leverage Adaptive Query Execution skew join handling (`spark.sql.adaptive.skewJoin.enabled = true`) (see [[learn-spark]]).

### Step 3: Check Partition Counts and Sizes
- Evaluate the partition count of the affected stage:
  - **Too few partitions**: Task input sizes exceed 200–500 MiB, causing memory spills and underutilized cores (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).
  - **Too many partitions / Small Files**: Tasks finish in milliseconds, but total stage duration is dominated by task scheduling overhead, driver listing delays, and shuffle file tracking (often caused by the [[parquet-small-files-problem]]) (see [[learn-spark]], [[apache-parquet]]).
- *Remedy*: Adjust `spark.sql.shuffle.partitions`, coalesce small file outputs, or tune input partition splits (see [[spark-partitioning-and-shuffle]], [[query-io-optimizations]]).

### Step 4: Check Join and Lakehouse Mutation Strategies
- Run `df.explain()` or view the Spark UI SQL execution graph (see [[learn-spark]]):
  - Check whether a standard join is performing a `SortMergeJoin` (involving an `Exchange` shuffle step) or a `BroadcastHashJoin`.
  - For Lakehouse updates (`MERGE INTO` in Iceberg or Delta Lake), recognize that mutations compile into full shuffle joins between incoming batches and target data files (see [[apache-iceberg]], [[iceberg-table-optimizations]]).
- *Remedies*:
  - If joining a large fact dataset with a small dimension table (< 10–50 MiB), wrap the dimension table in `broadcast()` (e.g., `df_large.join(broadcast(df_dim), "id")`) to bypass shuffle exchange entirely (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).
  - If `MERGE INTO` is slow, check merge key distribution for data skew, ensure partition pruning keys are present in the `ON` clause, and compact un-compacted Merge-On-Read delete files (see [[iceberg-table-optimizations]], [[iceberg-table-maintenance]]).

### Step 5: Check Spill and Memory Symptoms
- If memory-to-disk spills are detected:
  - **Filter and select early**: Drop unused columns and filter out unneeded rows before the shuffle stage to maximize Parquet column pruning and predicate pushdown (see [[learn-spark]], [[query-io-optimizations]]).
  - **Pre-aggregate**: Ensure partial aggregation takes place so raw record counts are reduced before crossing executor boundaries (see [[learn-spark]]).
  - **Increase parallelism**: Raise partition counts to reduce per-task memory footprints (see [[spark-partitioning-and-shuffle]]).
  - **Clean up caching**: Avoid unnecessary `.cache()` calls that consume storage memory and starve execution memory pools (see [[learn-spark]]).

## Mindset Shift: From User to Spark Engineer

Diagnosing performance requires translating vague complaints into concrete technical hypotheses (see [[learn-spark]]):

| Naive Observation | Diagnostic Root Cause | Actionable Fix |
| :--- | :--- | :--- |
| *"Spark is running slow on my group by."* | The `groupBy` triggers a shuffle exchange, and shuffle partitions are oversized or skewed. | Tune `spark.sql.shuffle.partitions` or inspect key distribution (see [[spark-partitioning-and-shuffle]]). |
| *"The join is taking forever."* | The join is executing as a `SortMergeJoin` with cross-network exchanges instead of broadcasting. | Verify dimension table size and enforce `broadcast()` (see [[learn-spark]]). |
| *"Tasks keep failing with out-of-memory or spilling."* | Partition sizes exceed execution memory headroom during aggregation. | Filter early, project required columns, and increase shuffle partition count (see [[spark-partitioning-and-shuffle]], [[query-io-optimizations]]). In AWS Glue, scale up worker type to G.2X/G.4X (see [[aws-glue-architecture-and-dpu]]). |
| *"All tasks finished except one stuck at 99%."* | Data skew: a single partition key contains an excessive concentration of records. | Apply key salting or enable AQE skew join optimization (see [[learn-spark]]). Avoid adding workers in serverless environments as it multiplies DPU costs with zero speedup (see [[aws-glue-job-optimization]]). |

## Pre-Execution Prediction Habit

Before submitting a PySpark pipeline to production, inspect your code and predict execution characteristics (see [[learn-spark]]):
1. Exactly where will shuffle `Exchange` boundaries be established?
2. Are join inputs small enough to support broadcast joins?
3. Will filtering or un-coalesced stages create thousands of tiny files? (see [[parquet-small-files-problem]])
4. At which stage will memory pressure peak, and is spill likely?
5. In serverless engines (AWS Glue), what is the DPU concurrency ceiling, and could skew cause idle worker cost bleed? (see [[aws-glue-architecture-and-dpu]], [[aws-glue-job-optimization]])

## Related

- [[learn-spark]]
- [[apache-parquet]]
- [[query-io-optimizations]]
- [[parquet-small-files-problem]]
- [[apache-iceberg]]
- [[iceberg-table-optimizations]]
- [[iceberg-table-maintenance]]
- [[aws-glue]]
- [[aws-glue-architecture-and-dpu]]
- [[aws-glue-job-optimization]]
- [[spark-partitioning-and-shuffle]]
- [[spark-execution-model]]
- [[apache-spark]]
