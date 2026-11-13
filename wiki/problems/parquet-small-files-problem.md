---
type: problem
title: The Parquet Small Files Problem
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-parquet]]", "[[learn-spark]]", "[[apache-iceberg]]"]
tags: [parquet, spark, storage, optimization, small-files, compaction, data-lake]
status: stable
---

# The Parquet Small Files Problem

The **Small Files Problem** occurs when an analytical dataset is fragmented into thousands or millions of tiny Parquet files (ranging from a few kilobytes to several megabytes), far below the recommended 128 MB to 1 GB threshold (see [[apache-parquet]]). This fragmentation degrades query engine performance, exhausts driver memory, overloads cloud object storage APIs, and negates the compression and scanning advantages of columnar storage (see [[apache-parquet]], [[spark-partitioning-and-shuffle]]).

## Root Causes of Small Files

Small files typically originate from three common architectural mistakes (see [[apache-parquet]], [[learn-spark]]):

1. **Over-Partitioning by High-Cardinality Columns**:
   - Creating nested physical partition directories based on granular or high-cardinality keys (e.g., `partitionBy("user_id")` or `partitionBy("year", "month", "day", "hour")`) (see [[apache-parquet]]).
   - When each directory contains only a handful of records, Spark produces thousands of near-empty files across partition folders (see [[apache-parquet]]).
2. **Un-Coalesced Writes After Wide Transformations**:
   - By default, PySpark operations (like `groupBy` or `join`) set `spark.sql.shuffle.partitions = 200` (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).
   - If a pipeline filters a dataset down to 10 MB and directly calls `.write.parquet()`, Spark writes across all 200 active partition tasks, generating 200 tiny files of ~50 KB each (see [[learn-spark]]).
3. **High-Frequency Streaming and Micro-Batch Ingestion**:
   - Streaming architectures that write micro-batches (e.g., every 10–60 seconds) directly to Parquet tables produce tens of thousands of tiny append files daily (see [[apache-parquet]]).

## Impact on Storage and Compute Engines

The accumulation of small files cripples distributed query performance across multiple architectural tiers (see [[apache-parquet]], [[learn-spark]]):

- **Query Planner & Driver Memory Bloat**:
  - Before executing a query, the Spark Driver must list all partition directories and read the footer metadata of every individual Parquet file (see [[apache-parquet]], [[apache-spark]]).
  - Processing footers for millions of files leads to extensive metadata discovery delays, heavy JVM garbage collection, and frequent Driver Out-Of-Memory (`Driver OOM`) errors (see [[learn-spark]]).
- **Task Scheduling Overhead & Thread Saturation**:
  - Spark typically allocates at least one task per physical file split (see [[learn-spark]]).
  - 50,000 small files spawn 50,000 tasks. The Driver spends significantly more time scheduling, dispatching, and serializing task metadata than executors spend reading data, resulting in sub-optimal core utilization (see [[learn-spark]], [[spark-execution-model]]).
- **Cloud Object Storage Throttling (S3 / GCS)**:
  - Reading thousands of small files issues an avalanche of `GET` and `LIST` API requests to object storage (see [[apache-parquet]]).
  - This frequently triggers cloud rate-limiting (`503 Slow Down`), adds network latency overhead, and drives up cloud API billing costs.
- **Destruction of Columnar & Compression Efficiencies**:
  - Parquet derives its compression power from large, homogeneous column chunks within 128–512 MB row groups (see [[parquet-file-architecture]]).
  - Tiny files contain tiny row groups where dictionary encoding and RLE cannot achieve high compression ratios, inflating the physical storage footprint (see [[apache-parquet]]).

## Prevention and Compaction Strategies

Resolving the small files problem requires proactive in-pipeline sizing combined with automated background compaction (see [[apache-parquet]], [[learn-spark]]):

### 1. In-Pipeline Coalescing (PySpark Prevention)
Always adjust partition counts immediately prior to invoking `.write.parquet()` (see [[learn-spark]], [[spark-partitioning-and-shuffle]]):

```python
# Calculate optimal partitions based on output size (target: ~256 MiB per file)
target_file_size_mb = 256
total_size_mb = estimated_dataset_size_mb
num_partitions = max(1, int(total_size_mb / target_file_size_mb))

# Use coalesce to reduce partition tasks without triggering an expensive shuffle
df.coalesce(num_partitions).write.mode("overwrite").parquet("s3://bucket/output/")
```
- **Rule**: Use `coalesce()` rather than `repartition()` when reducing partition count after filters to avoid a needless full cluster shuffle (see [[learn-spark]], [[spark-partitioning-and-shuffle]]).

### 2. Scheduled Batch Compaction Jobs
For tables subjected to regular incremental appends:
- Schedule a periodic maintenance job (e.g., daily or hourly) that reads small files from recent partition directories and rewrites them in consolidated chunks (see [[apache-parquet]]):
  ```python
  # Read fragmented partition data and write back consolidated
  (spark.read.parquet("s3://bucket/table/date=2026-11-13/")
        .coalesce(4)
        .write.mode("overwrite")
        .parquet("s3://bucket/table/date=2026-11-13_compacted/"))
  ```

### 3. Modern Lakehouse Table Formats (Delta Lake & Iceberg)
Because raw Parquet is immutable and lacks atomic multi-file replacement, modern architectures run open table formats over Parquet to automate compaction (see [[apache-parquet]], [[apache-parquet-format]], [[apache-iceberg]]):
- **Delta Lake**: Run the `OPTIMIZE` command to automatically bin-pack small files into 1 GB files:
  ```sql
  OPTIMIZE orders WHERE date = '2026-11-13';
  ```
- **Apache Iceberg**: Run the `rewrite_data_files` Spark procedure to dynamically bin-pack fragmented data files into balanced 128 MB – 512 MB targets without table locks (see [[apache-iceberg]], [[iceberg-table-maintenance]]):
  ```sql
  CALL glue_catalog.system.rewrite_data_files('sales_db.orders');
  ```

## Related

- [[apache-parquet]]
- [[parquet-file-architecture]]
- [[query-io-optimizations]]
- [[apache-parquet-format]]
- [[apache-iceberg]]
- [[iceberg-table-maintenance]]
- [[iceberg-table-optimizations]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
