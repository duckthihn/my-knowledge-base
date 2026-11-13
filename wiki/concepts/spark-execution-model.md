---
type: concept
title: Spark Execution Model and DAG
created: 2026-11-13
updated: 2026-11-13
sources: ["[[learn-spark]]"]
tags: [spark, pyspark, dag, execution-model, architecture]
status: stable
---

# Spark Execution Model and DAG

Apache Spark executes computations lazily by constructing a Directed Acyclic Graph (DAG) of transformations, which is converted into physical execution units only when an action is triggered (see [[learn-spark]]). The execution hierarchy decomposes into Jobs, Stages, and Tasks, with stage boundaries fundamentally defined by data shuffles (see [[learn-spark]]).

## Execution Hierarchy: Job → Stage → Task

Spark pipelines instructions logically before executing them physically (see [[learn-spark]]):

1. **Job**:
   - Each action called on a DataFrame (such as `.write.parquet()`, `.count()`, or `.collect()`) spawns exactly one Spark Job (see [[learn-spark]]).
   - The Job contains the entire DAG of transformations required to produce the requested output.

2. **Stage**:
   - Jobs are split into distinct Stages at **wide transformation boundaries** (operations that require data to be shuffled across nodes, such as `groupBy` or `join`) (see [[learn-spark]]).
   - Within a Stage, consecutive **narrow transformations** (such as `filter` and `select`) are pipelined together into a single execution stream without moving data across partitions or cluster nodes (see [[learn-spark]]).

3. **Task**:
   - Each Stage is broken down into a set of Tasks.
   - Exactly **one task processes one partition** of the data (see [[learn-spark]]).
   - Tasks are dispatched by the Driver directly to individual Executors for execution (see [[apache-spark]]).

```
Action (e.g. write) ──> Job ──> Split into Stages at Shuffle Boundaries ──> Tasks (1 per Partition)
```

## Narrow vs. Wide Transformations

The classification of transformations directly dictates execution performance and runtime cost (see [[learn-spark]]):

- **Narrow Transformations**:
  - Operations where each input partition contributes to at most one output partition (e.g., `filter`, `select`, `withColumn`).
  - Require no inter-node communication or network transfer.
  - Spark collapses multiple narrow operations into a single stage, avoiding intermediate disk writes (see [[learn-spark]]).

- **Wide Transformations**:
  - Operations where input partitions must contribute to multiple output partitions (e.g., `groupBy`, `join`, `distinct`, `orderBy`).
  - Introduce an `Exchange` operator in the physical execution plan, forcing a network shuffle and creating a new Stage boundary (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).
  - Wide transformations are the primary performance inflection points where latency, memory spills, and task stragglers originate (see [[debugging-slow-spark-jobs]]).

## Optimization Implications for PySpark

Understanding DAG boundaries shifts debugging from trial-and-error to targeted optimization (see [[learn-spark]]):

- **Look for Plan Operators**: Inspect the physical plan (`df.explain()`) for `Exchange` (shuffle boundary), `SortMergeJoin` (wide join), and `BroadcastHashJoin` (narrow join optimization) (see [[learn-spark]]).
- **Pipeline Operations Before Shuffling**: Ensure that heavy filtering and column selection occur within the narrow stage *before* wide transformations to minimize the volume of data crossing the network (see [[spark-partitioning-and-shuffle]]).
- **Anticipate Stage Bottlenecks**: Because each shuffle creates a new stage, excessive stages often indicate redundant shuffles or un-optimized join strategies (see [[debugging-slow-spark-jobs]]).

## Related

- [[learn-spark]]
- [[spark-partitioning-and-shuffle]]
- [[apache-spark]]
- [[debugging-slow-spark-jobs]]
