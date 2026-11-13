---
type: source
title: Spark Fundamentals - Day 1 Notes
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [spark, pyspark, performance, optimization, architecture]
status: stable
---

# Spark Fundamentals: Day 1 Notes

- **Original Title**: Spark Fundamentals: Day 1 Notes (Spark relearn)
- **Author / Source**: [Substack Post 207997209](https://substack.com/home/post/p-207997209), [Substack Post 189222561](https://substack.com/home/post/p-189222561), [YouTube Video](https://www.youtube.com/watch?v=aKYDDn4wU8c)
- **Path in raw**: `raw/learn-spark.md`

## Summary

A foundational yet optimization-centric guide to Apache Spark execution mechanics, covering the Job-Stage-Task hierarchy, DAG generation, partitioning mechanics, shuffle bottlenecks, cluster architecture, and a 5-step diagnostic checklist for slow PySpark queries.

## Key Takeaways

- **Action-Job-Stage-Task Boundary**: Actions create Jobs, which Spark compiles into a Directed Acyclic Graph (DAG). Stages are delineated strictly by wide transformations requiring shuffles (`groupBy`, `join`, `orderBy`), while narrow operations (`filter`, `select`) are pipelined within the same stage. Each task processes one partition within a stage.
- **Partitioning Strategy**: Spark splits DataFrames into input partitions (governed by data source split size, e.g., 128–256 MiB) and shuffle partitions (governed by configs and operations like `repartition` and `coalesce`). Balancing partition counts avoids memory spills (too few partitions) and scheduling overhead (too many partitions).
- **Shuffle as the Primary Bottleneck**: Wide dependencies trigger cross-node data transfers involving disk serialization, network I/O, and remote reads. Key symptoms of shuffle issues include stalled stages after joins/aggregations, disk spills, and straggler tasks caused by data skew.
- **PySpark Optimization Remedies**: Reduce shuffle overhead by filtering and selecting columns early, employing broadcast hash joins on small tables, tuning `spark.sql.shuffle.partitions` or enabling AQE, and using `coalesce` instead of `repartition` to shrink partition counts without a full shuffle.
- **Architecture Distinction**: Worker machines (e.g., EC2 instances) are long-lived infrastructure, distinct from ephemeral application-scoped Executors launched across those workers by the Master and coordinated directly by the Driver.
- **Systematic Debugging Workflow**: Instead of generic complaints, diagnose slow jobs via stage runtimes, task skew, partition distribution, join physical plan operators (`BroadcastHashJoin` vs. `SortMergeJoin`), and memory spill metrics.

## Notable Quotes

- *"Action → Job → chia Stage tại mỗi shuffle (wide transformation) → mỗi Stage gồm các Task, 1 task cho 1 partition."* (Context: Explaining how DAG boundaries split execution into parallel tasks).
- *"Those 'boundaries' matter. They’re where shuffles happen, where stages split, where time explodes."* (Context: Highlighting why understanding stage boundaries is essential for optimization).
- *"Spark never processes the DataFrame as one giant object. It always breaks it into multiple input partitions."* (Context: Clarifying the fundamental unit of work and parallelism).
- *"That’s the difference between a Spark user and a Spark engineer."* (Context: Moving from treating Spark as a black box to diagnosing execution plans and shuffle mechanics).

## Pages Created or Updated

- [[spark-execution-model]]: Core DAG structure, Job/Stage/Task lifecycle, and narrow vs. wide transformations.
- [[spark-partitioning-and-shuffle]]: Mechanics of input vs. shuffle partitions, `repartition` vs. `coalesce`, and shuffle reduction strategies.
- [[apache-spark]]: Spark cluster architecture, roles of Driver, Master, Worker, and Executor.
- [[debugging-slow-spark-jobs]]: 5-step diagnostic guide and optimization checklist for identifying skew, spills, and shuffle bottlenecks.

## Related

- [[spark-execution-model]]
- [[spark-partitioning-and-shuffle]]
- [[apache-spark]]
- [[debugging-slow-spark-jobs]]
