---
type: concept
title: AWS Glue Architecture and DPU Sizing
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-glue]]", "[[learn-spark]]"]
tags: [aws, glue, spark, dpu, architecture, cost-optimization, sizing]
status: stable
---

# AWS Glue Architecture and DPU Sizing

AWS Glue delivers a serverless execution runtime for Apache Spark where infrastructure provisioning, master node orchestration, and cluster lifecycle management are entirely managed by AWS (see [[aws-glue]]). Compute capacity is provisioned and billed using **Data Processing Units (DPUs)**, where optimizing worker selection and concurrency directly reduces AWS cloud expenditure (see [[aws-glue]], [[spark-execution-model]]).

## The Serverless Spark Mapping

In AWS Glue, each job execution maps directly to the standard Apache Spark cluster model (see [[aws-glue]], [[apache-spark]]):

| Apache Spark Primitive | AWS Glue Serverless Abstraction | Operational Behavior |
| :--- | :--- | :--- |
| **Spark Application** | **Job Run** | Created on trigger, terminates upon completion (see [[aws-glue]]) |
| **Cluster Master** | AWS Managed | Fully abstracted; no user configuration required (see [[aws-glue]]) |
| **Driver** | **1 Worker Node** | Dedicated worker executing the driver process and DAG scheduler (see [[aws-glue]]) |
| **Executors** | **Remaining Worker Nodes** | Each assigned worker hosts exactly one executor process (see [[aws-glue]]) |
| **Worker Hardware** | **DPU Allocation** | Defined by worker type (G.1X, G.2X, G.4X, G.8X) (see [[aws-glue]]) |

Because executors are ephemeral processes bound to a specific job run, cluster resources are deallocated the instant the script completes or terminates (see [[aws-glue]], [[apache-spark]]).

## DPU Specifications and Worker Types

A **Data Processing Unit (DPU)** is a standardized unit of compute capacity equivalent to **4 vCPUs and 16 GB of memory** (see [[aws-glue]]). AWS Glue provides several worker tiers tailored to distinct workload characteristics (see [[aws-glue]]):

| Worker Type | DPU per Worker | vCPU Cores | RAM | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **G.1X** | 1 DPU | 4 vCPU | 16 GB | Standard transforms, balanced joins, lightweight ETL (see [[aws-glue]]) |
| **G.2X** | 2 DPU | 8 vCPU | 32 GB | Memory-intensive jobs, wide joins, machine learning (see [[aws-glue]]) |
| **G.4X** | 4 DPU | 16 vCPU | 64 GB | Heavy aggregations, large shuffle partitions, spill relief (see [[aws-glue]]) |
| **G.8X** | 8 DPU | 32 vCPU | 128 GB | Extreme memory requirements, massive single-partition tasks (see [[aws-glue]]) |
| **G.025X** | 0.25 DPU | 2 vCPU | 4 GB | Low-volume streaming ingestion (Glue 3.0+) (see [[aws-glue]]) |

### Task Concurrency Formula
Understanding total concurrent task throughput is essential for balancing stage duration and task counts (see [[aws-glue]], [[spark-partitioning-and-shuffle]]):

$$\text{Active Executors} = \text{Total Workers} - 1 \quad (\text{1 worker reserved for the Driver})$$
$$\text{Max Concurrent Tasks} = \text{Active Executors} \times \text{vCPUs per Executor}$$

*Example*: A Glue job configured with **10 G.1X workers**:
- 1 worker runs the Driver.
- 9 workers run Executors (each providing 4 vCPUs).
- $\text{Max Concurrent Tasks} = 9 \times 4 = 36 \text{ tasks}$ in parallel (see [[aws-glue]]).
- If a Spark stage contains 100 partition tasks, execution will process in approximately 3 successive task waves (36, 36, and 28 tasks) (see [[aws-glue]], [[spark-execution-model]]).

## The DPU Sizing Decision Matrix

Choosing whether to scale worker types (scale up) or increase worker count (scale out) depends directly on execution symptoms (see [[aws-glue]], [[debugging-slow-spark-jobs]]):

```
Symptom: Memory Spills to Disk / Executor OOM
  ──> SCALE UP Worker Type (e.g., G.1X ──> G.2X or G.4X)
  ──> Increases RAM and execution memory pool per task

Symptom: High Stage Task Count / CPU Saturation
  ──> SCALE OUT Number of Workers
  ──> Adds executor cores to process task waves in parallel

Symptom: Data Skew (1 Task taking 50x longer at 99%)
  ──> DO NOT Add Workers
  ──> Extra workers sit idle; resolve skew via salting or AQE
```

- **Scale Up (Worker Type)**: When task metrics in the Spark UI display memory-to-disk spills or executors fail with `OutOfMemoryError`, moving from G.1X to G.2X doubles executor memory from 16 GB to 32 GB without changing core-to-memory ratios (see [[aws-glue]], [[spark-partitioning-and-shuffle]]).
- **Scale Out (Worker Count)**: When tasks are evenly sized and stages require dozens of execution waves, adding workers expands concurrency and reduces total stage runtime (see [[aws-glue]]).
- **The Skew Anti-Pattern**: Adding workers to resolve data skew is a costly error. All skewed records for a dominant key map to a single executor task; additional workers sit completely idle while the straggler task finishes (see [[aws-glue]], [[debugging-slow-spark-jobs]]).

## Financial Model and Execution Modes

Glue jobs are billed down to the second based on consumed DPU-hours (see [[aws-glue]]):

$$\text{Financial Cost} = \text{Allocated DPUs} \times \text{Runtime (hours)} \times \text{Hourly DPU Rate}$$

Because cost is the direct product of capacity and duration, eliminating shuffle bottlenecks, skew, and disk spills directly lowers infrastructure costs (see [[aws-glue]], [[aws-glue-job-optimization]]).

### Execution Modes
1. **Standard Execution**:
   - Immediate cluster startup on dedicated AWS hardware.
   - Designed for time-sensitive, SLA-bound production data pipelines (see [[aws-glue]]).
2. **Flex Execution**:
   - Runs Spark jobs on surplus, non-dedicated AWS compute capacity at a **30–50% cost discount** (see [[aws-glue]]).
   - Startup times may vary, and workers can be reclaimed if AWS requires capacity.
   - Ideal for non-urgent overnight batch jobs, historical data backfills, and testing (see [[aws-glue]]).
3. **Auto-Scaling**:
   - Available since Glue 3.0; dynamically adds and removes workers between 1 and a configured maximum based on the Spark stage task backlog (see [[aws-glue]]).
   - Prevents over-provisioning during light preliminary stages while providing maximum concurrency during heavy shuffle stages (see [[aws-glue]], [[aws-glue-job-optimization]]).

## Related

- [[aws-glue]]
- [[aws-glue-platform]]
- [[aws-glue-job-optimization]]
- [[dynamicframe-vs-dataframe]]
- [[glue-data-catalog]]
- [[spark-execution-model]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
- [[apache-spark]]
