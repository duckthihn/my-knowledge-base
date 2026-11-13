---
type: entity
title: Delta Lake
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]", "[[apache-parquet]]"]
tags: [delta-lake, entity, table-format, lakehouse, databricks, acid, open-source]
status: stable
---

# Delta Lake

Delta Lake is an open-source lakehouse table format developed by Databricks that layers ACID transactions, schema enforcement, and time travel over Apache Parquet data files stored on cloud object storage (see [[apache-parquet-format]], [[apache-iceberg]]). It is the default table format in the Databricks Lakehouse Platform and closely integrates with Apache Spark.

## Core Mechanics

Delta Lake manages table state through a JSON/Parquet **transaction log** (`_delta_log/`) stored alongside data files in object storage (see [[apache-parquet-format]]):

- Each write operation appends a new JSON commit entry to `_delta_log/`, recording the files added or removed.
- Readers reconstruct the current table state by replaying the log from the latest checkpoint forward.
- The log provides **ACID semantics** (Atomicity, Consistency, Isolation, Durability) without requiring a central catalog for atomic commits — a design difference from Apache Iceberg's catalog-pointer model (see [[iceberg-metadata-architecture]]).

## Key Capabilities

| Capability | Implementation |
|---|---|
| **ACID Transactions** | Serializable isolation via optimistic concurrency on the `_delta_log` |
| **Time Travel** | Query prior versions with `VERSION AS OF n` or `TIMESTAMP AS OF` |
| **Schema Enforcement** | Rejects writes that violate the declared table schema |
| **Schema Evolution** | `MERGE SCHEMA` option allows controlled column additions |
| **Upserts / Deletes** | `MERGE INTO`, `UPDATE`, `DELETE` via Copy-On-Write or Merge-On-Read |
| **Compaction** | `OPTIMIZE` command bin-packs small files into ~1 GB Parquet files (see [[parquet-small-files-problem]]) |
| **Deletion Vectors** | Compact Roaring bitmaps replacing positional delete files for fast row-level deletes |

## Comparison with Apache Iceberg

Delta Lake and Apache Iceberg share similar foundational goals but differ in governance model and partition flexibility (see [[apache-iceberg-format]], [[lakehouse-architecture]]):

| Feature | Delta Lake | Apache Iceberg |
|---|---|---|
| **ACID Mechanism** | `_delta_log` JSON/Parquet entries | Atomic catalog pointer swap (CAS) |
| **Engine Independence** | High (Spark, Trino, Flink; optimized for Databricks) | Exceptional (broadest vendor support) |
| **Partition Evolution** | Limited (requires rewrite to change strategy) | Native, zero data rewrites |
| **Primary Ecosystem** | Databricks Lakehouse Platform | Vendor-neutral open standard |
| **Deletion Vectors** | Yes (Roaring bitmaps) | Yes (v3, same binary spec — interoperable) |

> Format Convergence: Databricks encodes Delta Lake deletion vectors using the identical binary specification as Iceberg Format v3, enabling bidirectional interoperability between the two formats without data copying (see [[apache-iceberg-format]]).

## Small File Compaction

Delta Lake provides the `OPTIMIZE` command to consolidate fragmented small Parquet files into optimal large files (see [[parquet-small-files-problem]]):

```sql
-- Compact all small files in a date partition
OPTIMIZE orders WHERE date = '2026-11-13';

-- With Z-ORDER clustering for query pruning on non-partition columns
OPTIMIZE orders ZORDER BY (customer_id, region);
```

`Z-ORDER` co-locates related values within the same data files, tightening min/max statistics for multi-column predicate pushdown (see [[query-io-optimizations]]).

## Related

- [[lakehouse-architecture]]
- [[apache-iceberg-format]]
- [[apache-parquet-format]]
- [[parquet-small-files-problem]]
- [[iceberg-table-optimizations]]
- [[iceberg-metadata-architecture]]
- [[query-io-optimizations]]
- [[spark-execution-model]]
