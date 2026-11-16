---
type: source
title: AWS Athena - Serverless Interactive Query Service
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [aws, athena, sql, trino, serverless, query-optimization, cost-optimization, s3]
status: stable
---

# AWS Athena: Serverless Interactive Query Service

- **Original Title**: What is Athena / How it works, Cost, Optimization & Partition Projection
- **Author / Source**: AWS Documentation & Practical Notes (`raw/aws-athena.md`), [AWS Athena Pricing](https://aws.amazon.com/vi/athena/pricing/)
- **Path in raw**: `raw/aws-athena.md`

## Summary

A comprehensive architectural and performance guide to Amazon Athena, AWS's serverless interactive query service based on open-source Trino. The source explains how Athena executes distributed ANSI SQL directly against Amazon S3 data lakes using the AWS Glue Data Catalog. It details Athena's pay-per-scan pricing model ($5.00 per TB scanned), contrasts its zero-provisioning serverless model with Apache Spark on AWS Glue, and establishes why physical data layout is the sole optimization lever for reducing query latency and cost. It also details the mechanics, configuration, and operational gotchas of Partition Projection.

## Key Takeaways

- **Serverless Trino Architecture**: Athena runs managed open-source Trino (MPP distributed SQL query engine) across an AWS-managed coordinator and worker fleet. Unlike Spark on Glue, there are no clusters, drivers, or workers to provision. Because users cannot adjust cluster compute, **physical data layout and SQL syntax are the sole performance and cost levers**.
- **Financial Model: Cost = Data Scanned**: Athena charges **$5.00 per TB of data scanned** from S3 (minimum 10 MB per query; DDL, cancelled, and failed queries are free). Charges depend entirely on bytes scanned rather than rows returned.
- **Data Layout Dictates Query Cost**: Converting uncompressed text files (CSV/JSON) into compressed columnar formats (Apache Parquet with Snappy/ZSTD) reduces scanned bytes and query bills by 90–95% (e.g., scanning a single column on a 3 TB dataset drops from $15.00 down to $1.25).
- **Primary Optimization Levers**: Enforce column pruning (ban `SELECT *`), compress datasets, partition by high-frequency filter keys, size files between 128 MB and 512 MB to avoid small file opening overhead, and activate Query Result Reuse to eliminate redundant query scans.
- **Partition Projection for Scalability**: Solves Glue Data Catalog metadata lookup latency on tables with tens or hundreds of thousands of partitions. By defining deterministic mathematical rules (`date`, `integer`, `enum`, `injected`) and S3 location templates in table properties, Athena calculates target S3 URIs dynamically from `WHERE` predicates without making catalog calls.
- **Partition Projection Caveats**: Partition projection is an Athena-proprietary feature (Redshift Spectrum and EMR do not recognize projected partitions without manual catalog entries), misconfigured templates fail silently by returning 0 rows, and it should never be enabled on Apache Iceberg tables.

## Notable Quotes

- *"Dữ liệu Parquet trên S3 và một table trong Glue Data Catalog, viết SQL, và có kết quả mà không dựng gì cả. Đây là điểm khác lớn nhất với Spark trên Glue, phải viết job, chọn worker và chờ job khởi động."* (Context: Contrasting Athena's immediate interactive query model with Spark job provisioning).
- *"Về mặt kiến trúc, Trino là MPP... bạn không điều chỉnh được cluster, nên đòn bẩy duy nhất của bạn là cách bố trí dữ liệu và cách viết SQL."* (Context: Highlighting why data layout is the central performance lever in Athena).
- *"Chi phí = lượng dữ liệu quét, nên layout dữ liệu chính là chi phí (partition, file format, …)."* (Context: Establishing the fundamental financial principle of Athena).
- *"Partition projection đảo ngược cách làm: bạn mô tả quy luật của các partition trong thuộc tính của bảng, và Athena tự suy ra các đường dẫn S3 từ quy luật đó cùng với điều kiện WHERE, không cần hỏi catalog."* (Context: Explaining the architectural paradigm shift of partition projection).

## Pages Created or Updated

- [[aws-athena-architecture-and-cost]]: Trino MPP distributed query model, $5/TB scanned billing mechanics, query lifecycle, and data layout cost impacts.
- [[athena-partition-projection]]: Overcoming Glue Catalog metadata bottlenecks, configuration types (`date`, `enum`, `integer`, `injected`), S3 URI templates, and operational caveats.
- [[athena-query-optimization-and-cost]]: Practical checklist for cutting Athena query costs and latency: Parquet column pruning, compression, partition pruning, file consolidation, and result reuse.
- [[aws-athena-service]]: Platform overview, Trino lineage, integration with S3, Glue Data Catalog, Lake Formation, and Apache Iceberg.
- [[query-io-optimizations]]: Updated to connect Parquet column pruning directly to Athena's $5/TB scanned pricing model.
- [[glue-data-catalog]]: Updated with details on how Athena queries catalog tables and why Partition Projection is used for massive partition counts.
- [[apache-iceberg-format]]: Updated with details on Athena querying Iceberg tables natively without partition projection.

## Related

- [[aws-athena-architecture-and-cost]]
- [[athena-partition-projection]]
- [[athena-query-optimization-and-cost]]
- [[aws-athena-service]]
- [[trino]]
- [[query-io-optimizations]]
- [[glue-data-catalog]]
- [[parquet-file-architecture]]
- [[parquet-small-files-problem]]
- [[apache-iceberg-format]]
