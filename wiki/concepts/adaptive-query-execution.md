---
type: concept
title: Adaptive Query Execution (AQE)
created: 2026-11-13
updated: 2026-11-13
sources: ["[[learn-spark]]", "[[aws-glue]]"]
tags: [spark, aqe, optimization, shuffle, skew, partitioning, performance]
status: stable
---

# Adaptive Query Execution (AQE)

**Adaptive Query Execution (AQE)** is a Spark runtime optimization framework (introduced in Spark 3.0, stable since Spark 3.2) that re-optimizes query plans *during execution* based on accurate runtime statistics collected after each shuffle stage (see [[learn-spark]]). Unlike static query planning — which estimates partition sizes and join strategies before any data is read — AQE reacts to actual observed data distributions, automatically correcting misestimates that would otherwise cause data skew, excessive shuffle partitions, or suboptimal join strategies (see [[learn-spark]], [[spark-execution-model]]).

## Enabling AQE

```python
# Enable in PySpark (default: true in Spark 3.2+)
spark.conf.set("spark.sql.adaptive.enabled", "true")

# Enable skew join handling (default: true when AQE is enabled)
spark.conf.set("spark.sql.adaptive.skewJoin.enabled", "true")
```

In AWS Glue jobs, AQE is available from Glue 3.0+ and can be set via Spark configuration properties (see [[aws-glue-architecture-and-dpu]]).

## The Three Core AQE Optimizations

### 1. Dynamic Shuffle Partition Coalescing

**Problem**: The static `spark.sql.shuffle.partitions` default (200) is frequently wrong — a small dataset after heavy filtering produces 200 near-empty shuffle partitions, causing excessive task scheduling overhead and tiny output files (see [[spark-partitioning-and-shuffle]], [[parquet-small-files-problem]]).

**AQE Fix**: After each shuffle stage completes, AQE inspects actual partition sizes and **coalesces consecutive small partitions** into larger ones before the next stage begins:

```
Before AQE: 200 shuffle partitions (many near-empty after filter)
                ↓ AQE coalesces at runtime
After AQE:  12 balanced shuffle partitions (each ~256 MB)
```

- Eliminates thousands of near-empty tasks without manual tuning of `spark.sql.shuffle.partitions`.
- Directly reduces the number of small output Parquet files written to S3 (see [[parquet-small-files-problem]]).

### 2. Dynamic Skew Join Handling

**Problem**: Data skew — where one join key contains disproportionately many rows (e.g., a single `null` value or dominant country code) — routes all matching records to a single executor task, creating a "straggler" that takes 50×–100× longer than other tasks while all other workers sit idle (see [[debugging-slow-spark-jobs]], [[aws-glue-job-optimization]]).

**AQE Fix**: AQE detects skewed partitions (using configurable size and row count thresholds) and **splits them into smaller sub-tasks**, joining each sub-task against a replicated slice of the other side:

```
Skewed partition detected (key="US", 10M rows vs. median 50K rows)
   ↓ AQE splits into 20 sub-partitions
   ↓ Each sub-partition joins against a replicated 50K-row slice
   ↓ 20 balanced tasks run in parallel (no straggler)
```

- Eliminates the most common cause of Spark job stalls without manual key salting.
- Configuration: `spark.sql.adaptive.skewJoin.skewedPartitionThresholdInBytes` (default: 256 MB).

### 3. Dynamic Join Strategy Switching

**Problem**: The query planner estimates join input sizes before execution. If the estimate is wrong (e.g., a heavy filter reduces one join side from 500 MB to 2 MB at runtime), the planner may choose an expensive `SortMergeJoin` instead of a cheap `BroadcastHashJoin` (see [[spark-execution-model]], [[spark-partitioning-and-shuffle]]).

**AQE Fix**: After observing actual shuffle stage output sizes, AQE can **dynamically demote a `SortMergeJoin` to a `BroadcastHashJoin`** if one side falls below `spark.sql.autoBroadcastJoinThreshold` (default: 10 MB):

```
Planned: SortMergeJoin (estimated 500 MB on both sides)
   ↓ After shuffle stage: right side = 3 MB actual
   ↓ AQE switches to BroadcastHashJoin at runtime
Result: Eliminates the shuffle Exchange on the large side entirely
```

## AQE vs. Manual Tuning

| Scenario | Manual Fix | AQE Fix |
|---|---|---|
| Too many small shuffle partitions | Tune `spark.sql.shuffle.partitions` | Auto-coalesces after each stage |
| Data skew on join key | Key salting (manual code change) | Auto-splits skewed partitions |
| Wrong join strategy | Wrap small table in `broadcast()` | Auto-switches to broadcast if size drops |
| Over-partitioned output files | `df.coalesce(n)` before write | Reduces upstream shuffle output automatically |

> [!TIP] AQE does not replace all manual tuning — it is most effective when statistics are uncertain at plan time. For known-skewed keys or intentional broadcasts, explicit hints (`broadcast()`, salting) remain valuable and predictable.

## AQE in AWS Glue

In AWS Glue's serverless billing model, AQE is especially valuable because idle workers during skew still consume DPU-hours (see [[aws-glue-job-optimization]], [[aws-glue-architecture-and-dpu]]):

- **Skew at 99%**: Adding more G.1X workers to fix a straggler task costs more without fixing skew. Enable `spark.sql.adaptive.skewJoin.enabled` instead.
- **Auto-Scaling + AQE**: Together, AQE's partition coalescing reduces stage parallelism on small stages, allowing Auto-Scaling to release workers and reduce billed DPU-hours (see [[aws-glue-architecture-and-dpu]]).

## Related

- [[learn-spark]]
- [[spark-partitioning-and-shuffle]]
- [[spark-execution-model]]
- [[debugging-slow-spark-jobs]]
- [[aws-glue-job-optimization]]
- [[aws-glue-architecture-and-dpu]]
- [[parquet-small-files-problem]]
