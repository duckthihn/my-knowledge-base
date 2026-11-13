---
type: entity
title: Apache Parquet Format
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-parquet]]"]
tags: [parquet, entity, open-source, storage-format, ecosystem, big-data]
status: stable
---

# Apache Parquet Format

Apache Parquet is an open-source, engine-agnostic columnar storage format designed specifically for complex bulk analytics and distributed big data processing (see [[apache-parquet]]). Originating around 2013 as a joint open-source project between Twitter and Cloudera, Parquet was engineered around the principles published in Google's Dremel paper ("Dremel: Interactive Analysis of Web-Scale Datasets") to provide high-performance columnar representation for nested data structures (see [[apache-parquet]]).

## Origins and Ecosystem Interoperability

Parquet emerged to solve the severe I/O and CPU bottlenecks of text-based formats (CSV, JSON) and row-based binary formats (Avro) when executing large-scale analytical (OLAP) queries (see [[apache-parquet]]):

- **Cross-Engine Standard**: Parquet is a first-class storage standard across the data ecosystem, natively supported by:
  - **Distributed Compute Engines**: Apache Spark, Trino, Presto, Apache Hive, Apache Flink (see [[apache-parquet]], [[apache-spark]]).
  - **Embedded / Local Engines**: DuckDB, Pandas, Polars, PyArrow (see [[apache-parquet]]).
  - **Cloud Data Warehouses & Lakehouse Query Services**: Snowflake, Google Cloud BigQuery, AWS Athena, Databricks (see [[apache-parquet]]).

## Comparison with Alternative Storage Formats

Different storage formats serve distinct operational workloads (see [[apache-parquet]]):

| Criterion | CSV / JSON | Apache Avro | Apache Parquet |
| :--- | :--- | :--- | :--- |
| **Storage Orientation** | Row-based | Row-based | Columnar (see [[parquet-file-architecture]]) |
| **Human Readable** | Yes | No (Binary) | No (Binary) |
| **Schema Evolution** | Brittle / Inferred | Full schema in header | Self-describing footer metadata |
| **Compression Ratio** | Poor | Moderate | Very High (homogeneous types per page) |
| **OLAP Read Performance** | Slow (full scans) | Moderate | Ultra-Fast (pruning & pushdown) |
| **Single-Record Streaming** | Moderate | Excellent (append-friendly) | Poor (immutable row groups) |

## The Foundation for Lakehouse Table Formats

While Parquet provides exceptional read and compression characteristics, individual Parquet files are fundamentally **immutable** (see [[apache-parquet]]):
- Updating or deleting a single row requires rewriting the entire Parquet file or row group (see [[apache-parquet]]).
- Raw Parquet lacks native multi-file ACID transaction coordination, isolation levels, and historical point-in-time querying (time travel) (see [[apache-parquet]]).

To overcome these constraints in modern data lakehouses, modern open table formats are constructed on top of Parquet (see [[apache-parquet]]):
- **Delta Lake**: Maintains a JSON/Parquet transaction log (`_delta_log`) alongside Parquet data files, adding ACID guarantees, upserts (`MERGE INTO`), and schema enforcement.
- **Apache Iceberg**: Organizes Parquet data files via atomic metadata trees (manifest lists and manifest files), providing snapshot isolation, partition evolution, and fast compaction.
- **Apache Hudi**: Provides copy-on-write and merge-on-read table primitives using Parquet data files paired with Avro log files for near-real-time ingestion.

## Related

- [[apache-parquet]]
- [[parquet-file-architecture]]
- [[query-io-optimizations]]
- [[parquet-small-files-problem]]
- [[apache-spark]]
- [[spark-partitioning-and-shuffle]]
