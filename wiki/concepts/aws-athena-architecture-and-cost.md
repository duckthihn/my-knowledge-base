---
type: concept
title: AWS Athena Architecture and Cost Model
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-athena]]", "[[apache-parquet]]"]
tags: [aws, athena, trino, mpp, serverless, cost-optimization, query-planning]
status: stable
---

# AWS Athena Architecture and Cost Model

Amazon Athena is a serverless, interactive query service built on open-source **Trino** (formerly PrestoSQL) that executes distributed ANSI SQL directly against data stored in Amazon S3 (see [[aws-athena]]). Unlike traditional cluster-based engines like Apache Spark, Athena abstracts all cluster management, making **physical data layout on S3 and SQL construction the sole optimization levers** for controlling query latency and cloud expenditure (see [[aws-athena]], [[athena-query-optimization-and-cost]]).

## The Serverless Trino Architecture

Athena operates on a Massively Parallel Processing (MPP) architecture where execution workers and coordinators are fully managed by AWS (see [[aws-athena]]):

```
Client SQL Query
   │
   ▼
[Trino Coordinator (AWS Managed)]
   │ 1. Fetches metadata, schemas, and partition paths from AWS Glue Data Catalog
   │ 2. Compiles query DAG and evaluates column pruning and partition pruning
   │ 3. Decomposes execution into parallel stage splits
   ▼
[Trino Worker Fleet (AWS Managed)]
   │ 4. Workers read split data blocks directly from S3 in parallel
   │ 5. Perform in-memory filtering, hashing, joins, and aggregations
   ▼
[S3 Query Results Bucket]
   │ 6. Writes final result set (CSV) to s3://.../query-results/
   ▼
Client Receives Query Result Set
```

### The "No Cluster Tuning" Reality
In compute engines like AWS Glue or Amazon EMR, engineers tune infrastructure parameters: DPU worker tiers, executor memory pools, core allocations, and shuffle partition counts (see [[aws-glue-architecture-and-dpu]], [[spark-partitioning-and-shuffle]]). 

In Athena, the coordinator, worker fleet, and inter-node networking are completely abstracted (see [[aws-athena]]):
- You cannot add memory to workers to resolve spills.
- You cannot scale out worker counts manually to process large stages faster.
- **Consequence**: Engineering efficiency shifts entirely to **upstream data layout** (format selection, compression codecs, partition design, file sizing) and **SQL query design** (see [[aws-athena]], [[athena-query-optimization-and-cost]]).

## The Financial Formula: Cost = Data Scanned

Athena's standard pricing model charges strictly based on the volume of physical data scanned from Amazon S3 (see [[aws-athena]]):

$$\text{Query Financial Cost} = \text{Data Scanned (TB)} \times \$5.00$$

### Key Billing Rules
- **Granularity**: Billed per megabyte scanned, rounded up to the nearest megabyte, with a **10 MB minimum per query** (see [[aws-athena]]).
- **Free Operations**: DDL queries (`CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`, `SHOW TBLPROPERTIES`), failed queries, and user-cancelled queries incur **$0.00** in compute charges (see [[aws-athena]]).
- **Zero Idle Expense**: No ongoing infrastructure baseline cost; charges occur only during active query execution (see [[aws-athena]]).

### Data Scanned vs. Data Returned
A common architectural trap is assuming Athena charges based on the size of the final result set (see [[aws-athena]]):
- Athena bills for the **raw bytes transferred from S3 into Trino workers**, completely independent of the rows returned (see [[aws-athena]]).
- *Example*: Executing `SELECT COUNT(*)` on an un-partitioned 5 TB CSV dataset scans the entire 5 TB of data from S3, incurring a **$25.00 charge**, even though the output returned to the client is a single 8-byte integer (see [[aws-athena]]).

## Direct Impact of Data Layout on Billing

The physical layout of datasets on Amazon S3 directly determines whether a query scans petabytes or megabytes (see [[aws-athena]], [[query-io-optimizations]]):

| Storage Layout | Raw Dataset Size | Scanned Data for 1 Column | Query Cost | Efficiency Multiplier |
| :--- | :--- | :--- | :--- | :--- |
| **Uncompressed CSV** | 3.0 TB | 3.0 TB (Row format requires scanning all bytes) (see [[aws-athena]]) | **$15.00** | 1x (Baseline) |
| **Gzip Compressed CSV** | 1.0 TB | 1.0 TB (Must decompress full row stream) (see [[aws-athena]]) | **$5.00** | 3x Savings |
| **Uncompressed Parquet** | 3.0 TB | 0.4 TB (Column pruning skips unreferenced columns) (see [[parquet-file-architecture]]) | **$2.00** | 7.5x Savings |
| **ZSTD Compressed Parquet** | 0.8 TB | **0.15 TB** (Column pruning + columnar page compression) (see [[aws-athena]]) | **$0.75** | **20x Savings** |

Converting raw row-oriented data into compressed columnar Parquet yields a direct **20x cost reduction** for analytical queries because Trino workers seek directly to specific column chunk byte offsets in S3 without reading unreferenced attributes (see [[parquet-file-architecture]], [[query-io-optimizations]]).

## Related

- [[aws-athena]]
- [[athena-query-optimization-and-cost]]
- [[athena-partition-projection]]
- [[aws-athena-service]]
- [[trino]]
- [[parquet-file-architecture]]
- [[query-io-optimizations]]
- [[parquet-small-files-problem]]
- [[glue-data-catalog]]
