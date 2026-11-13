---
type: topic
title: Lakehouse Architecture
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]", "[[apache-parquet]]", "[[aws-glue]]", "[[aws-lake-formation]]", "[[aws-athena]]"]
tags: [lakehouse, architecture, iceberg, parquet, data-lake, data-warehouse, aws]
status: stable
---

# Lakehouse Architecture

The **lakehouse** is an open architecture that combines the low-cost, flexible storage of a data lake (object storage like S3) with the ACID transactions, schema enforcement, and performance optimizations traditionally reserved for data warehouses. It achieves this by layering a **table format** (such as [[apache-iceberg-format|Apache Iceberg]]) over columnar **file formats** (such as [[apache-parquet-format|Apache Parquet]]) managed through a central **catalog** (see [[apache-iceberg]], [[aws-glue]]).

## The Three-Layer Stack

A lakehouse cleanly separates three responsibilities that legacy architectures conflated (see [[apache-iceberg-format]]):

```
┌──────────────────────────────────────────────────────────┐
│  COMPUTE LAYER                                           │
│  Apache Spark, Trino, AWS Athena, Flink, DuckDB          │
│  (see [[apache-spark]], [[aws-athena-service]])          │
└──────────────────────────┬───────────────────────────────┘
                           │ reads/writes via
                           ▼
┌──────────────────────────────────────────────────────────┐
│  TABLE FORMAT LAYER                                      │
│  Apache Iceberg · Delta Lake · Apache Hudi               │
│  → ACID transactions, snapshot isolation, time travel    │
│  → Schema & partition evolution                          │
│  → File discovery via metadata (no S3 LIST)             │
│  (see [[apache-iceberg-format]], [[iceberg-metadata-architecture]]) │
└──────────────────────────┬───────────────────────────────┘
                           │ manages
                           ▼
┌──────────────────────────────────────────────────────────┐
│  FILE FORMAT + OBJECT STORAGE LAYER                      │
│  Apache Parquet / ORC / Avro  on  AWS S3 / GCS / ADLS   │
│  → Columnar compression, row-group min/max statistics    │
│  (see [[parquet-file-architecture]], [[apache-parquet-format]]) │
└──────────────────────────────────────────────────────────┘
```

Each layer is independently swappable: Spark can be replaced with Trino, Parquet with ORC, and AWS Glue Catalog with Project Nessie — without touching other layers (see [[apache-iceberg-format]]).

## Why Lakehouse? The Problems It Solves

### Legacy Hive-Style Data Lakes

Traditional S3 data lakes stored data as `s3://bucket/year=2026/month=11/` directory trees (Hive-style partitioning). This created structural problems (see [[apache-iceberg]]):

- **No ACID**: Partial writes were immediately visible to readers — a job crash could expose half-written data.
- **Expensive Discovery**: Query engines issued millions of `LIST` API calls to discover files in deeply nested directories, adding seconds to query planning.
- **Brittle Schema**: Column names or positions encoded in directory paths broke historical reads on any schema change.
- **Rigid Partitions**: Changing from monthly to daily partitioning required a full table rewrite.

### Lakehouse Solutions

| Problem | Lakehouse Solution |
|---|---|
| No ACID | Snapshot isolation via atomic metadata pointer swap (OCC) — see [[iceberg-metadata-architecture]] |
| Expensive file discovery | Table defined by metadata files, not directories — zero `LIST` calls |
| Brittle schema | Column IDs (integer-keyed, not name/position) — rename/drop/reorder without file rewrites |
| Rigid partitions | Partition evolution: old files stay monthly-partitioned, new appends go daily |
| No time travel | Snapshot history retained for rollback and audit |

## Apache Iceberg: The Open Standard

[[apache-iceberg-format|Apache Iceberg]] has emerged as the dominant open table format for lakehouses due to its exceptional engine neutrality and partition evolution capabilities (see [[apache-iceberg-format]]):

### Iceberg Metadata Architecture (3-Tier)

Iceberg manages tables through a hierarchical metadata tree (see [[iceberg-metadata-architecture]]):

1. **Catalog** (AWS Glue, Nessie, REST): Holds the atomic pointer to the current `*.metadata.json` file. Supports Compare-And-Swap (CAS) for ACID commits.
2. **Metadata Layer**:
   - *Metadata Files*: Table schema, partition spec, snapshot log.
   - *Manifest Lists*: One per snapshot; lists all manifest files.
   - *Manifest Files*: Lists all data/delete files with column `[min, max]` statistics.
3. **Data Layer**: Immutable Parquet data files and delete files.

### Two-Tier Query Pruning

Iceberg chains metadata-level pruning with Parquet-level pruning to minimize I/O (see [[iceberg-table-optimizations]], [[query-io-optimizations]]):

- **Tier 1 — Manifest Pruning**: Column min/max stats in manifest files prune entire data files before opening them (e.g., 10,000 files → 40 candidates).
- **Tier 2 — Parquet Row-Group Pushdown**: Parquet footer stats prune row groups within surviving files (see [[parquet-file-architecture]]).

### Row-Level Mutations

Two write strategies handle `UPDATE`, `DELETE`, and `MERGE INTO` (see [[iceberg-table-optimizations]]):

| Strategy | Write Speed | Read Speed | Best For |
|---|---|---|---|
| **Copy-On-Write (COW)** | Slow (rewrites full files) | Fast | Read-heavy analytical tables |
| **Merge-On-Read (MOR)** | Fast (append delete files) | Slower over time | CDC, high-frequency updates |
| **Format v3 Deletion Vectors** | Fast (bitmap append) | Fast | Best of both — requires Iceberg 1.11.0+ |

## The AWS Lakehouse Stack

On AWS, the lakehouse pattern converges on a well-defined set of managed services (see [[aws-glue]], [[aws-lake-formation]], [[aws-athena]]):

```
┌──────────────────────────────────────────────────────────┐
│  GOVERNANCE                                              │
│  AWS Lake Formation — column/row-level access control,  │
│  data sharing, LF-Tags ABAC                             │
│  (see [[lake-formation-security-model]])                 │
└──────────────────────────┬───────────────────────────────┘
                           │ enforces policy on
                           ▼
┌──────────────────────────────────────────────────────────┐
│  CATALOG                                                 │
│  AWS Glue Data Catalog — Iceberg atomic commit pointer  │
│  (see [[glue-data-catalog]])                             │
└──────────────────────────┬───────────────────────────────┘
                           │
          ┌────────────────┼──────────────────┐
          ▼                ▼                  ▼
   ┌─────────────┐  ┌─────────────┐  ┌──────────────┐
   │  AWS Glue   │  │  Amazon     │  │  Amazon EMR  │
   │  ETL Jobs   │  │  Athena     │  │  / Spark     │
   │ (PySpark)   │  │ (Presto)    │  │              │
   │ see         │  │ see         │  │ see          │
   │[[aws-glue]] │  │[[aws-athena]]│  │[[apache-spark]]│
   └──────┬──────┘  └──────┬──────┘  └──────┬───────┘
          └────────────────┼─────────────────┘
                           │ read/write
                           ▼
              ┌──────────────────────┐
              │  Amazon S3           │
              │  Iceberg Tables      │
              │  (Parquet files)     │
              └──────────────────────┘
```

**Key integrations:**
- **AWS Glue Crawlers are unnecessary** for Iceberg tables — schema and partitions are discovered directly from Iceberg metadata (see [[apache-iceberg-format]], [[glue-data-catalog]]).
- **Athena Partition Projection is unnecessary** — Iceberg manifest pruning handles the same optimization natively (see [[athena-partition-projection]]).
- **Lake Formation** enforces fine-grained access control on top of Iceberg tables without changing the data layout (see [[lake-formation-security-model]]).

## Table Format Comparison

| Feature | Apache Iceberg | Delta Lake | Apache Hudi | Hive-Style |
|---|---|---|---|---|
| **ACID Transactions** | Yes (OCC, snapshot isolation) | Yes (ACID log) | Yes (Timeline log) | No |
| **Time Travel** | Yes | Yes | Yes | No |
| **Engine Independence** | Exceptional | High (Databricks-optimized) | Good | High (legacy) |
| **Partition Evolution** | Native, zero rewrites | Limited | Limited | Impossible |
| **Streaming / CDC** | Good (MOR + v3 DVs) | Good | Excellent | No |
| **Primary Strength** | Open standard, multi-engine | Databricks ecosystem | Near-realtime CDC | Simplicity |

> [!NOTE] Format Convergence
> Databricks encodes Format v3 deletion vectors in Iceberg using the same binary spec as Delta Lake, enabling bidirectional interoperability without data copying (see [[apache-iceberg-format]]).

## When to Use a Lakehouse (vs. Alternatives)

**Use the lakehouse pattern when:**
- Multiple engines (Spark, Athena, Flink, Trino) must access the same data
- ACID semantics are required on object storage (CDC, MERGE INTO, upserts)
- Schema or partition strategy will evolve over time
- Audit trails, time travel, and rollback are required
- Scale exceeds what a managed warehouse (Redshift, BigQuery) can cost-effectively handle

**Consider alternatives when:**
- Pure Databricks shop → Delta Lake (deeper integration)
- Near-realtime streaming CDC is the dominant workload → Apache Hudi
- Simple analytics on stable, small datasets → Plain Parquet + Glue Catalog
- Full managed warehouse convenience is prioritized over cost → Redshift Serverless / BigQuery

## Operational Requirements

Lakehouses (specifically Iceberg) require scheduled maintenance — they do **not** self-compact (see [[iceberg-table-maintenance]]):

| Procedure | Purpose | Cadence |
|---|---|---|
| `rewrite_data_files` | Compact small files → 128–512 MB, merge delete files | Daily / Weekly |
| `expire_snapshots` | Reclaim S3 storage from old snapshots | Daily / Weekly |
| `remove_orphan_files` | Delete uncommitted files from failed jobs | Weekly |

> [!CAUTION]
> Never delete Iceberg data or metadata files directly via AWS CLI or S3 SDK — this corrupts the manifest tree and causes `FileNotFoundException` on subsequent queries (see [[iceberg-table-maintenance]]).

## Related

- [[apache-iceberg]]
- [[apache-iceberg-format]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[iceberg-table-maintenance]]
- [[parquet-file-architecture]]
- [[apache-parquet-format]]
- [[query-io-optimizations]]
- [[glue-data-catalog]]
- [[lake-formation-security-model]]
- [[aws-athena-architecture-and-cost]]
- [[aws-glue-architecture-and-dpu]]
- [[spark-execution-model]]
