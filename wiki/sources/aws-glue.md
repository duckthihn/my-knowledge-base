---
type: source
title: AWS Glue - Architecture, Jobs, and Serverless Data Integration
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [aws, glue, serverless, spark, pyspark, dpu, cost-optimization, etl]
status: stable
---

# AWS Glue: Architecture, Jobs, and Serverless Data Integration

- **Original Title**: What is Glue / Glue Job, DPU, Data Catalog, Workflows & Triggers
- **Author / Source**: AWS Documentation & Practical Notes (`raw/aws-glue.md`), [AWS Glue Pricing](https://aws.amazon.com/vi/glue/pricing/)
- **Path in raw**: `raw/aws-glue.md`

## Summary

A comprehensive architectural and performance guide to AWS Glue, Amazon's serverless data integration and ETL platform. The notes examine how Glue abstracts Apache Spark clusters into serverless Data Processing Units (DPUs), maps driver and executor allocations to worker types (G.1X to G.8X), and calculates billing via DPU-hours. It explores differences between Spark DataFrames and Glue DynamicFrames, details the Data Catalog and partition pruning via `push_down_predicate`, highlights small file packing via `groupFiles`, and outlines pipeline orchestration using Workflows, Triggers, and run properties while emphasizing cost optimization and anti-pattern avoidance.

## Key Takeaways

- **Serverless Spark Mapping & DPU Sizing**: A Glue Job run is an ephemeral Spark application. The Master and cluster manager are managed by AWS; 1 worker is allocated to the Driver, and remaining workers host 1 Executor each. Resources are sized in DPUs (1 DPU = 4 vCPU + 16 GB RAM). Worker types (G.1X, G.2X, G.4X, G.8X) define memory per executor and cores per task.
- **Direct Financial Impact of Optimization**: Glue pricing is calculated as $\text{Cost} = \text{DPUs} \times \text{Runtime (hours)}$. Performance bottlenecks such as shuffle disk spills, data skew, un-coalesced small files, and driver memory bottlenecks directly inflate cloud bills.
- **DynamicFrame vs. DataFrame**: DynamicFrame supports schema-on-read flexibility, tracking schemas per record and resolving dirty data via `choice` types and `resolveChoice()`, but bypasses Spark's Catalyst optimizer. Best practice follows a hybrid model: ingest dirty data with DynamicFrame, clean and resolve types, then convert to DataFrame (`.toDF()`) for Catalyst-optimized execution.
- **Two-Level Partitioning & I/O Pruning**: Catalog partitions reflect physical S3 directory hierarchies (`year=/month=`), while Spark partitions are in-memory units of execution. Using `push_down_predicate` prunes S3 partitions during catalog listing *before* Spark scans files, while `groupFiles` bundles thousands of small files into balanced partitions on read.
- **Orchestration & Consolidation**: Glue Workflows manage dependencies across jobs and triggers (`SCHEDULED`, `CONDITIONAL`, `EVENT`), passing parameters via workflow run properties. Chaining multiple small jobs composed only of narrow transformations is an anti-pattern that pays repeated cluster startup latency and intermediate S3 I/O.
- **Production Schema Contracts**: While Crawlers infer schemas for exploratory data, production pipelines and modern lakehouse formats (e.g., Apache Iceberg) bypass Crawlers in favor of explicit DDL contracts or native table metadata.

## Notable Quotes

- *"Một cách nghĩ hữu ích: tiền = số DPU × thời gian chạy, nên job chậm do shuffle hay skew đều tốn tiền trực tiếp."* (Context: Explaining the direct monetary cost of Spark performance issues in serverless Glue).
- *"Job: 10 worker G.1X → thường 1 worker làm driver, 9 worker còn lại là executor (mỗi cái 4 core) → 9 executor × 4 core = 36 task chạy song song."* (Context: Formulating worker allocation and task parallelism in Glue).
- *"Tăng worker để chữa skew: tốn tiền mà không nhanh hơn, vì một task vẫn gánh phần lệch."* (Context: Highlighting the major anti-pattern of scaling cluster resources for data skew).
- *"Nguyên tắc: các bước chỉ gồm narrow transformation nối tiếp thường nên gộp vào cùng một job. Chỉ tách job khi có lý do rõ ràng."* (Context: Pipeline orchestration efficiency and cluster startup cost reduction).

## Pages Created or Updated

- [[aws-glue-architecture-and-dpu]]: Serverless Spark cluster mapping, DPU sizing (G.1X to G.8X), task parallelism formulas, Flex vs. Standard modes, and the DPU cost model.
- [[dynamicframe-vs-dataframe]]: Mechanics of DynamicFrame, `choice` type resolution, Catalyst optimizer trade-offs, and recommended hybrid pipeline patterns.
- [[glue-data-catalog]]: Centralized metadata store, external table semantics, Catalog vs. Spark partitioning, `push_down_predicate`, and `groupFiles`.
- [[aws-glue-job-optimization]]: Optimization checklist for Glue: worker sizing for memory/spills, skew mitigation, `groupFiles`, job bookmarks, timeout safety, and pipeline consolidation.
- [[aws-glue-platform]]: Entity overview of the AWS Glue platform, core sub-services, and ecosystem integration.
- [[spark-partitioning-and-shuffle]]: Updated with Glue worker-to-task concurrency formulas and `groupFiles` small file packing.
- [[debugging-slow-spark-jobs]]: Updated with CloudWatch/Spark UI monitoring and DPU financial cost analysis.
- [[apache-iceberg-format]]: Updated with details on Glue Data Catalog integration and crawler elimination.

## Related

- [[aws-glue-architecture-and-dpu]]
- [[dynamicframe-vs-dataframe]]
- [[glue-data-catalog]]
- [[aws-glue-job-optimization]]
- [[aws-glue-platform]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
- [[apache-iceberg-format]]
