---
type: concept
title: Iceberg Metadata Architecture and ACID Commits
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]"]
tags: [iceberg, metadata, acid, transactions, architecture, catalog]
status: stable
---

# Iceberg Metadata Architecture and ACID Commits

Apache Iceberg organizes data lake tables through a hierarchical metadata tree rather than physical directory structures on object storage (see [[apache-iceberg]]). By defining a table strictly as the set of files registered in its current metadata snapshot, Iceberg provides full ACID transactions, snapshot isolation, and instantaneous file discovery without performing expensive filesystem listing calls (see [[apache-iceberg]]).

## The 3-Tier Architectural Hierarchy

An Iceberg table decouples storage into three organized architectural layers (see [[apache-iceberg]]):

```
┌────────────────────────────────────────────────────────┐
│ 1. CATALOG LAYER                                       │
│    (AWS Glue, Nessie, Hive Metastore)                  │
│    Holds current metadata pointer: s3://.../v3.json   │
└──────────────────────────┬─────────────────────────────┘
                           │ Points to current version
                           ▼
┌────────────────────────────────────────────────────────┐
│ 2. METADATA LAYER                                      │
│    a. Metadata Files (v1.json, v2.json, v3.json)       │
│       - Schema, Partition Spec, Snapshot Log           │
│                           │ Current Snapshot           │
│                           ▼                            │
│    b. Manifest List (snap-101.avro)                    │
│       - List of manifest files + partition ranges      │
│                           │                            │
│                           ▼                            │
│    c. Manifest Files (manifest-a.avro, ...)            │
│       - List of Data Files (Parquet)                   │
│       - Column min/max statistics, file status         │
└──────────────────────────┬─────────────────────────────┘
                           │ Direct file references
                           ▼
┌────────────────────────────────────────────────────────┐
│ 3. DATA LAYER                                          │
│    Data Files (file-1.parquet, file-2.parquet)         │
│    Delete Files (positional / equality / bitmaps)      │
└────────────────────────────────────────────────────────┘
```

### 1. The Catalog Layer
The Catalog serves as the entry point for every read and write operation (see [[apache-iceberg]]):
- Stores the single "pointer" locating the current active metadata file (e.g., `s3://bucket/metadata/v3.metadata.json`) (see [[apache-iceberg]]).
- Must support **atomic compare-and-swap (CAS) operations** (see [[apache-iceberg]]). When a transaction commits, the catalog atomically swings the pointer to the new metadata version.
- Common catalog implementations include AWS Glue Data Catalog, Project Nessie, Hive Metastore, and REST catalogs (see [[apache-iceberg]], [[apache-iceberg-format]]).

### 2. The Metadata Layer
The metadata layer maintains a complete, linear history of table mutations across three distinct file types (see [[apache-iceberg]]):

1. **Metadata Files (`*.metadata.json`)**:
   - Stores the complete table schema, partition specifications, current snapshot ID, and a chronological log of previous snapshots (see [[apache-iceberg]]).
   - Whenever table schema or configuration changes, a new metadata file is created (see [[apache-iceberg]]).
2. **Manifest Lists (`snap-*.avro`)**:
   - Captures the exact state of an Iceberg table at a specific point in time (a snapshot) (see [[apache-iceberg]]).
   - Contains an array of structs pointing to all active **Manifest Files** for that snapshot, along with partition summaries and manifest boundaries to optimize planning (see [[apache-iceberg]]).
3. **Manifest Files (`*.avro`)**:
   - Tracks individual physical **Data Files** and **Delete Files** (see [[apache-iceberg]]).
   - Records essential file-level statistics, including exact S3 file paths, format (e.g., Parquet), row counts, and column-level minimum and maximum values (see [[apache-iceberg]], [[query-io-optimizations]]).

### 3. The Data Layer
Contains the actual underlying analytical files stored in object storage (see [[apache-iceberg]]):
- **Data Files**: Immutable columnar files (primarily Parquet) containing actual table rows (see [[apache-parquet-format]], [[parquet-file-architecture]]).
- **Delete Files**: Files tracking deleted or updated records for Merge-On-Read (MOR) operations (see [[iceberg-table-optimizations]]).

## Read and Write Lifecycle

### The Read Path
Query planning completes without issuing a single filesystem `LIST` command (see [[apache-iceberg]]):
1. **Catalog Lookup**: The query engine queries the catalog to locate the latest table metadata file (see [[apache-iceberg]]).
2. **Snapshot Resolution**: The engine reads the metadata file and identifies the current active snapshot, opening its corresponding Manifest List (see [[apache-iceberg]]).
3. **Manifest Evaluation & File Pruning**: The engine reads manifest files, evaluating the query filter against column `[min, max]` statistics to prune non-matching data files (see [[apache-iceberg]], [[query-io-optimizations]]).
4. **Data Scan**: The engine reads only the surviving physical data files from S3, pushing down remaining row-group predicates into the Parquet reader (see [[apache-parquet]], [[query-io-optimizations]]).

### The Write Path & Optimistic Concurrency Control (OCC)
Writes achieve ACID transactions through isolated staging and atomic commits (see [[apache-iceberg]]):
1. **Isolated Data Write**: The writer writes new immutable Parquet data files to S3. Existing files are never modified in-place (see [[apache-iceberg]]).
2. **Metadata Construction**: The writer generates new manifest files and a new metadata file capturing the table state including the newly written files (see [[apache-iceberg]]).
3. **Atomic Pointer Swap**: The writer attempts to update the catalog pointer to the new metadata file (see [[apache-iceberg]]).
4. **Optimistic Concurrency Control (OCC)**:
   - If no other commit occurred during execution, the atomic CAS succeeds, and the commit becomes immediately visible to readers (see [[apache-iceberg]]).
   - If another writer committed first, a conflict check evaluates whether the concurrent transactions overlap. If non-conflicting, the writer rebases and retries; if conflicting, the commit fails safely without leaving partial data exposed (see [[apache-iceberg]], [[iceberg-table-maintenance]]).

## Related

- [[apache-iceberg]]
- [[iceberg-table-optimizations]]
- [[apache-iceberg-format]]
- [[iceberg-table-maintenance]]
- [[query-io-optimizations]]
- [[parquet-file-architecture]]
- [[apache-parquet-format]]
