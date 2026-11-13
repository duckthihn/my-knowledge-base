---
type: concept
title: Query I/O Optimizations in Columnar Storage
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-parquet]]", "[[learn-spark]]", "[[apache-iceberg]]", "[[aws-athena]]"]
tags: [parquet, spark, optimization, predicate-pushdown, column-pruning, performance, sorting]
status: stable
---

# Query I/O Optimizations in Columnar Storage

Query I/O optimizations in columnar storage minimize disk read latency, network throughput bottlenecks, and memory footprint by discarding irrelevant records and attributes before data is transferred to engine execution memory (see [[apache-parquet]], [[spark-execution-model]]). In Apache Parquet, this is achieved through two complementary mechanisms: **Column Pruning** (vertical filtering) and **Predicate Pushdown** (horizontal filtering), heavily amplified by disciplined **data sorting strategies** on write (see [[apache-parquet]]).

## Column Pruning (Vertical Filtering)

In traditional row-based stores (like CSV or Avro), reading a single column still requires scanning all byte offsets for every row on disk (see [[apache-parquet]]).

- **Mechanics in Parquet**:
  - Parquet isolates each column into dedicated Column Chunks within each Row Group (see [[parquet-file-architecture]]).
  - When a query specifies `.select("user_id", "amount")`, the query planner inspects the file footer to obtain the exact file byte offsets of those specific column chunks (see [[apache-parquet]]).
  - The reader seeks directly to those offsets, skipping all untouched columns entirely (see [[apache-parquet]]).
- **Performance & Cost Impact**:
  - For wide schemas (e.g., tables with 100+ columns), scanning 5 columns reduces physical disk and network I/O by 90–95%, eliminating memory pressure before transformations begin (see [[apache-parquet]], [[debugging-slow-spark-jobs]]).
  - In serverless pay-per-scan query services like [[aws-athena|Amazon Athena]], this reduction translates directly to a **90%–95% drop in query financial charges** ($5/TB scanned) (see [[aws-athena]], [[aws-athena-architecture-and-cost]]).

## Predicate Pushdown (Two-Tier Pruning)

Predicate pushdown pushes filter expressions (e.g., `WHERE status = 'active'` or `WHERE transaction_date >= '2026-01-01'`) down to the storage and metadata layers, pruning unneeded data blocks before they are decoded or transferred into the query engine (see [[apache-parquet]], [[learn-spark]], [[apache-iceberg]]).

In modern lakehouses, predicate pushdown operates across **two distinct tiers** (see [[iceberg-table-optimizations]]):
1. **Tier 1 (Manifest-Level File Pruning)**: Table formats (like Apache Iceberg) evaluate filter predicates against file-level min/max column bounds stored in manifest files, pruning non-matching data files before opening them from S3 (see [[apache-iceberg]], [[iceberg-table-optimizations]]).
2. **Tier 2 (Parquet Row-Group Pruning)**: For candidate files that survive Tier 1, the engine inspects the Parquet footer min/max statistics to skip individual row groups within each file (see [[apache-parquet]], [[parquet-file-architecture]]).

```
Query: SELECT amount FROM orders WHERE amount > 1000

Tier 1: Iceberg Manifest File Pruning
  Evaluate min/max per data file ──> Prune unneeded S3 data files entirely

Tier 2: Parquet Footer Metadata Evaluation (Surviving Files)
┌────────────────────────────────────────────────────────┐
│ Row Group 1: amount min = 5,    max = 850   ──> SKIP   │
│ Row Group 2: amount min = 200,  max = 2400  ──> SCAN   │
│ Row Group 3: amount min = 1200, max = 5000  ──> SCAN   │
└────────────────────────────────────────────────────────┘

Step 3: Seek directly to Row Groups 2 & 3 (Row Group 1 skipped entirely)
```

1. **Footer Statistics Evaluation**:
   - The Parquet file footer records summary statistics for every column chunk in every row group, including minimum value, maximum value, and null counts (see [[apache-parquet]], [[parquet-file-architecture]]).
   - When the reader encounters a filter predicate, it evaluates whether the target range intersects with the row group's `[min, max]` envelope (see [[apache-parquet]]).
2. **Coarse-Grained Pruning**:
   - If the predicate cannot be satisfied (e.g., filter is `amount > 1000`, but Row Group 1 has `max = 850`), the query engine skips the entire row group (see [[apache-parquet]]).
   - Neither the dictionary pages nor the data pages for that row group are read from storage or decompressed, drastically reducing CPU and I/O cycles (see [[apache-parquet]]).

## Sorting Strategies on Write: The Multiplier for Predicate Pushdown

Predicate pushdown depends entirely on the variance and distribution of values across row groups (see [[apache-parquet]]):

### The Unsorted Data Anti-Pattern
If data is written in arbitrary insertion order, each row group will contain a broad and overlapping spectrum of values:
- *Row Group 1*: `amount` min = 1, max = 10,000
- *Row Group 2*: `amount` min = 2, max = 9,800
- *Row Group 3*: `amount` min = 5, max = 10,000

Because every row group spans the full range, a predicate like `amount > 5000` matches the min/max criteria of every single row group. **Zero row groups are pruned**, forcing the engine to scan 100% of the dataset (see [[apache-parquet]]).

### The Sorted Data Strategy
Sorting data on write by primary query filter columns clusters identical and contiguous values into distinct row groups (see [[apache-parquet]]):
- *Row Group 1*: `amount` min = 1, max = 250
- *Row Group 2*: `amount` min = 251, max = 800
- *Row Group 3*: `amount` min = 801, max = 5,000
- *Row Group 4*: `amount` min = 5,001, max = 10,000

When querying `amount > 5000`, the engine instantly prunes Row Groups 1, 2, and 3 based on footer metadata, reading only Row Group 4 (see [[apache-parquet]]).

### Practical Sorting Best Practices in PySpark
- **Sort by High-Selectivity Columns**: Use `.sortWithinPartitions("filter_col")` or `.orderBy("date", "category")` prior to writing Parquet files to tighten min/max ranges (see [[learn-spark]], [[apache-parquet]]).
- **Avoid Over-Sorting**: Limit sorting to 1–2 primary filter columns; sorting on excessive low-correlation columns dilutes clustering effectiveness for all but the primary sort key.

## Sizing and Partitioning Alignment

To maximize I/O efficiency, align physical file sizing with computational parallelism (see [[apache-parquet]], [[spark-partitioning-and-shuffle]]):

1. **Target File Sizing (128 MB to 1 GB)**:
   - Sizing Parquet files between 128 MB and 1 GB ensures each file contains enough row groups (e.g., 128–512 MB each) to amortize footer metadata costs while providing optimal task partition sizes for Spark executors (see [[apache-parquet]], [[spark-partitioning-and-shuffle]]).
2. **Directory Partitioning vs. Row Group Filtering**:
   - **Directory Partitioning** (e.g., `s3://bucket/table/year=2026/month=11/`): Use strictly for low-cardinality attributes (dates, regions) to eliminate directory scans at the filesystem level (see [[apache-parquet]]).
   - **Row Group Filtering**: Rely on Parquet min/max statistics within sorted files for medium-to-high cardinality columns (e.g., `customer_id`, `status`), avoiding the severe metadata degradation of excessive directory hierarchies (see [[apache-parquet]], [[parquet-small-files-problem]]).

## Related

- [[apache-parquet]]
- [[parquet-file-architecture]]
- [[parquet-small-files-problem]]
- [[aws-athena]]
- [[aws-athena-architecture-and-cost]]
- [[athena-query-optimization-and-cost]]
- [[apache-iceberg]]
- [[iceberg-table-optimizations]]
- [[iceberg-metadata-architecture]]
- [[spark-partitioning-and-shuffle]]
- [[spark-execution-model]]
- [[debugging-slow-spark-jobs]]
