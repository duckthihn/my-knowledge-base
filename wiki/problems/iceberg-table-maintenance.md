---
type: problem
title: Iceberg Table Maintenance and Performance Degradation
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]", "[[learn-spark]]", "[[apache-parquet]]"]
tags: [iceberg, spark, maintenance, compaction, small-files, delete-files, optimization]
status: stable
---

# Iceberg Table Maintenance and Performance Degradation

While Apache Iceberg abstracts storage management and provides ACID transactions, it does not perform automated background compaction or file garbage collection (see [[apache-iceberg]]). Without scheduled maintenance, Iceberg tables suffer from progressive performance degradation caused by small data file fragmentation, delete file bloat in Merge-On-Read (MOR) tables, snapshot metadata explosion, and orphan file accumulation (see [[apache-iceberg]], [[parquet-small-files-problem]]).

## Symptoms of Table Degradation

An unmaintained Iceberg table displays distinct performance and cost symptoms across read and write workloads (see [[apache-iceberg]]):

1. **Read Scan Slowdowns (Small File & Delete File Bloat)**:
   - Frequent appends create thousands of tiny data files, leading to excessive task scheduling overhead and driver metadata parsing delays (see [[apache-iceberg]], [[parquet-small-files-problem]]).
   - On Format v2 Merge-On-Read tables, frequent updates generate dozens of positional delete files per partition. Engines must read and reconcile every delete file during scans, degrading query throughput over time (see [[apache-iceberg]], [[iceberg-table-optimizations]]).
2. **Monotonic Storage Cost Inflation (Snapshot Bloat)**:
   - Because Iceberg maintains complete snapshot histories for time travel, old data files are never deleted when rows are modified or dropped (see [[apache-iceberg]]).
   - If historical snapshots are never expired, physical S3 storage grows indefinitely, retaining obsolete and tombstoned data files (see [[apache-iceberg]]).
3. **Commit Conflicts under High Concurrency**:
   - Multiple writers updating the same partition simultaneously trigger Optimistic Concurrency Control (OCC) conflicts (see [[apache-iceberg]], [[iceberg-metadata-architecture]]).
   - If write jobs fail to commit before competing transactions finish, cascading retries overload the catalog and driver (see [[apache-iceberg]]).

## Routine Maintenance Procedures in PySpark

To maintain high query performance and manage storage costs, data platforms must execute three core Spark maintenance procedures on a scheduled cadence (e.g., daily or weekly) (see [[apache-iceberg]]):

### 1. Data File Compaction (`rewrite_data_files`)
Consolidates thousands of small, fragmented data files into optimal 128 MB to 512 MB Parquet files using bin-packing algorithms (see [[apache-iceberg]], [[parquet-small-files-problem]]):

```sql
-- Compact small data files in the orders table
CALL glue_catalog.system.rewrite_data_files(
    table => 'sales_db.orders',
    strategy => 'binpack',
    options => map(
        'target-file-size-bytes', '268435456', -- 256 MB
        'min-file-size-bytes', '134217728',    -- 128 MB
        'max-file-size-bytes', '536870912'     -- 512 MB
    )
);
```
- **Benefit**: Replaces thousands of tiny tasks with balanced, vectorized scan tasks, drastically reducing Spark planning and scan latency (see [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]]).

### 2. Snapshot Expiration (`expire_snapshots`)
Removes snapshots older than a configured data retention window (e.g., 7 or 30 days) and physically deletes underlying data and manifest files no longer referenced by any active snapshot (see [[apache-iceberg]]):

```sql
-- Expire snapshots older than 7 days
CALL glue_catalog.system.expire_snapshots(
    table => 'sales_db.orders',
    older_than => TIMESTAMP '2026-11-06 00:00:00'
);
```
- **Benefit**: Reclaims physical S3 storage and keeps the table's `*.metadata.json` file small, preventing driver memory exhaustion during metadata parsing (see [[apache-iceberg]], [[iceberg-metadata-architecture]]).

### 3. Orphan File Removal (`remove_orphan_files`)
Scans storage to detect and permanently delete data files that were written to S3 by failed, aborted, or crashed write jobs and were never committed to any snapshot (see [[apache-iceberg]]):

```sql
-- Remove uncommitted orphan files older than 3 days
CALL glue_catalog.system.remove_orphan_files(
    table => 'sales_db.orders',
    older_than => TIMESTAMP '2026-11-10 00:00:00'
);
```

## Critical Operational Anti-Patterns

Avoid these common operational pitfalls when managing Iceberg tables (see [[apache-iceberg]]):

- **Manual S3 Deletions**: Never delete data or metadata files directly via the AWS CLI or S3 SDK. Doing so corrupts the Iceberg manifest metadata tree, causing subsequent queries to fail with `FileNotFoundException` (see [[apache-iceberg]]).
- **Running MOR Without Regular Compaction**: If using Format v2 Merge-On-Read for high-frequency CDC, schedule hourly or daily compaction to merge delete files into base data files or adopt Format v3 Deletion Vectors (see [[apache-iceberg]], [[iceberg-table-optimizations]]).
- **Over-Partitioning**: Even with hidden partitioning, declaring overly fine-grained partitions (e.g., `hours(ts)`) produces millions of tiny files, recreating the small files problem (see [[apache-iceberg]], [[parquet-small-files-problem]]).
- **Premature Format v3 Upgrades**: Before upgrading a table property to `format-version = '3'`, verify that every compute engine reading and writing to the table (e.g., EMR, Glue, Athena, Redshift) fully supports v3 features like deletion vectors, or jobs will fail during commit (see [[apache-iceberg]]).

## Related

- [[apache-iceberg]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[apache-iceberg-format]]
- [[parquet-small-files-problem]]
- [[debugging-slow-spark-jobs]]
- [[spark-partitioning-and-shuffle]]
