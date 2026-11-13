---
type: concept
title: Iceberg Table Optimizations and Query Mechanics
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]", "[[learn-spark]]", "[[apache-parquet]]"]
tags: [iceberg, spark, optimization, pruning, partition-evolution, merge-on-read, deletion-vectors]
status: stable
---

# Iceberg Table Optimizations and Query Mechanics

Apache Iceberg introduces optimizations that eliminate physical filesystem bottlenecks, decoupling logical table design from physical file organization (see [[apache-iceberg]]). By combining two-tier query pruning, hidden partitioning, schema evolution via column IDs, and format v3 deletion vectors, Iceberg accelerates analytical queries and streamlines row-level data modifications (see [[apache-iceberg]], [[query-io-optimizations]]).

## Two-Tier Query Pruning

Iceberg dramatically reduces query scan overhead by evaluating filter predicates across two distinct metadata layers before reading raw data (see [[apache-iceberg]], [[query-io-optimizations]]):

```
Query: SELECT * FROM orders WHERE amount > 5000 AND order_ts >= '2026-11-01'

[TIER 1: Manifest Pruning]
  Engine inspects Manifest Files in metadata
  Evaluates column [min, max] statistics per data file
  ──> 10,000 Data Files pruned down to 40 candidate Data Files (0 data bytes read)

[TIER 2: Parquet Row Group Pushdown]
  Engine opens only the 40 candidate Data Files
  Evaluates Parquet Footer [min, max] statistics per Row Group
  ──> Prunes unneeded Row Groups within surviving files
  ──> Scans only relevant Column Chunks
```

1. **Tier 1 (Manifest-Level File Pruning)**:
   - Manifest files store lower and upper bounds, null counts, and NaN counts for all columns across every individual data file (see [[apache-iceberg]], [[iceberg-metadata-architecture]]).
   - The query engine prunes non-qualifying data files entirely at the metadata level, eliminating millions of unnecessary S3 `GET` requests (see [[apache-iceberg]]).
2. **Tier 2 (Parquet Row-Group Predicate Pushdown)**:
   - For files that pass Tier 1 pruning, the Parquet reader evaluates footer statistics to skip individual row groups within each file (see [[apache-parquet]], [[query-io-optimizations]]).

## Hidden Partitioning and Partition Evolution

Traditional Hive partitioning forces table consumers to know the physical directory partitioning scheme, leading to silent full-table scans when users omit partition columns from queries (see [[apache-iceberg]]):

### Hidden Partitioning
- Iceberg partitions data using logical transformation functions (e.g., `days(order_ts)`, `bucket(16, user_id)`, `identity(region)`) declared in the table schema (see [[apache-iceberg]]):
  ```sql
  CREATE TABLE sales.orders (
      order_id bigint, amount double, order_ts timestamp
  ) USING iceberg
  PARTITIONED BY (days(order_ts));
  ```
- **Transparent Filtering**: Users query the raw column (`WHERE order_ts >= '2026-11-01'`). Iceberg transparently converts the timestamp predicate into partition boundary pruning without requiring users to supply synthetic date partition strings (see [[apache-iceberg]]).

### Partition Evolution
- In traditional data lakes, changing a partition layout (e.g., transitioning from monthly to daily partitions) requires an expensive full-table rewrite (see [[apache-iceberg]]).
- Iceberg supports **partition evolution in-place** without rewriting historical data:
  - Each metadata snapshot tracks which partition specification was active when a data file was written (see [[apache-iceberg]], [[iceberg-metadata-architecture]]).
  - Old files remain partitioned by month, while new appends write under the daily specification. Queries span both transparently, applying appropriate pruning logic to each partition generation (see [[apache-iceberg]]).

## Safe Schema Evolution via Column IDs

Traditional formats identify columns by name (brittle) or ordinal position (breaking historical reads when columns are reordered or dropped) (see [[apache-iceberg]], [[apache-parquet-format]]):
- Iceberg assigns every column an immutable, unique **integer ID** (see [[apache-iceberg]]).
- Adding, renaming, dropping, or reordering columns only updates metadata mappings:
  - **Renaming a column** modifies the ID-to-name mapping without rewriting underlying Parquet files (see [[apache-iceberg]]).
  - **Dropping a column** removes the ID from future projections.
  - Old Parquet files containing previous column names continue to read correctly because the reader resolves data based on IDs rather than string column headers (see [[apache-iceberg]]).

## Row-Level Mutations: Copy-On-Write vs. Merge-On-Read

Iceberg provides two strategies for executing `UPDATE`, `DELETE`, and `MERGE INTO` operations (see [[apache-iceberg]]):

| Criterion | Copy-On-Write (COW) | Merge-On-Read (MOR) |
| :--- | :--- | :--- |
| **Write Mechanism** | Rewrites entire affected data files (see [[apache-iceberg]]) | Appends small delete files, leaving data files untouched (see [[apache-iceberg]]) |
| **Write Speed** | Slower (high write amplification) | Faster (append-only) |
| **Read Speed** | Fast (clean sequential scan) | Slower (reader merges deletes at scan time) |
| **Best For** | Read-heavy analytical tables, low-churn ETL | Write-heavy ingestion, Change Data Capture (CDC) |

### `MERGE INTO` Execution Mechanics in Spark
In PySpark, a `MERGE INTO` statement compiles into a **wide transformation join** between source updates and target Iceberg data files (see [[apache-iceberg]], [[spark-execution-model]]):
- Triggers a full shuffle exchange across worker nodes based on the merge key (see [[spark-execution-model]], [[spark-partitioning-and-shuffle]]).
- If merge keys are unbalanced, the operation suffers from severe **data skew**, creating straggler tasks in the shuffle stage (see [[learn-spark]], [[debugging-slow-spark-jobs]]).

## Iceberg Format v3 and Deletion Vectors

Iceberg Format v3 (production-ready since Iceberg 1.11.0 / May 2026) eliminates the primary bottleneck of Format v2 Merge-On-Read tables (see [[apache-iceberg]]):

- **The v2 Bottleneck**: In v2, frequent row deletes produce dozens of positional delete files. On read, the query engine must open and evaluate every delete file, causing read performance to degrade steadily until table compaction is executed (see [[apache-iceberg]], [[iceberg-table-maintenance]]).
- **Format v3 Deletion Vectors**:
  - Replaces discrete positional delete files with compact, compressed Roaring bitmaps (see [[apache-iceberg]]).
  - Provides near-instantaneous delete filtering during scans with negligible memory overhead (see [[apache-iceberg]]).
  - Shares binary encoding alignment with Delta Lake deletion vectors, facilitating cross-format interoperability (see [[apache-iceberg]], [[apache-iceberg-format]]).
- **Additional v3 Capabilities**: Native row lineage for CDC auditing, `VARIANT` data type for high-performance semi-structured JSON querying, default column values, and nanosecond timestamp precision (see [[apache-iceberg]]).

## Related

- [[apache-iceberg]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-maintenance]]
- [[apache-iceberg-format]]
- [[query-io-optimizations]]
- [[parquet-small-files-problem]]
- [[spark-execution-model]]
- [[debugging-slow-spark-jobs]]
