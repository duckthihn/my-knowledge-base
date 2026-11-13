---
type: concept
title: Parquet File Architecture and Internals
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-parquet]]"]
tags: [parquet, storage, columnar, encoding, compression, architecture]
status: stable
---

# Parquet File Architecture and Internals

Apache Parquet is an open-source columnar storage format designed to maximize I/O efficiency, data compression, and analytical throughput for big data processing (see [[apache-parquet]]). Originating in 2013 from a collaboration between Twitter and Cloudera based on Google's Dremel paper, Parquet organizes records into structured hierarchies of row groups, column chunks, and pages, accompanied by comprehensive metadata footers (see [[apache-parquet]], [[apache-parquet-format]]).

## Columnar Storage vs. Row-Based Layout

Traditional transactional file formats (such as CSV, JSON, and Apache Avro) store data row-by-row, writing all fields of record 1 contiguously before record 2 (see [[apache-parquet]]):

- **Row-Based Layout**: `(id_1, name_1, age_1), (id_2, name_2, age_2), ...`
- **Columnar Layout (Parquet)**: `(id_1, id_2, ...), (name_1, name_2, ...), (age_1, age_2, ...)`

This architectural orientation yields three critical advantages (see [[apache-parquet]]):
1. **Column Pruning**: Queries scanning specific columns (e.g., `SELECT AVG(age)`) completely bypass unreferenced column bytes on disk (see [[query-io-optimizations]]).
2. **Dense Data Compression**: Values within a single column chunk share the same data type and often exhibit repetitive patterns, resulting in substantially higher compression ratios than heterogeneous row streams (see [[apache-parquet]]).
3. **Vectorized Execution**: Modern query engines (such as Apache Spark, Trino, and DuckDB) process arrays of column values using SIMD CPU instructions without row-decoding overhead (see [[apache-parquet]], [[spark-execution-model]]).

## Internal File Hierarchy

A single Parquet file is organized into four hierarchical layers (see [[apache-parquet]]):

```
File
├── Row Group 1 (typically 128 MB – 512 MB, unit of parallelism)
│   ├── Column Chunk (Column A)
│   │   ├── Dictionary Page (optional)
│   │   ├── Data Page 1
│   │   └── Data Page 2
│   └── Column Chunk (Column B) ...
├── Row Group 2 ...
└── File Footer (Schema, Row Group metadata, Column chunk min/max statistics)
```

1. **Row Group**:
   - A horizontal partitioning of rows within the file, typically sized from tens to hundreds of megabytes (see [[apache-parquet]]).
   - Acts as the primary unit of coarse-grained parallelism for distributed execution engines like Spark (see [[spark-execution-model]], [[spark-partitioning-and-shuffle]]).
2. **Column Chunk**:
   - The contiguous byte sequence for a specific column within a given row group (see [[apache-parquet]]).
   - Contains all the pages for that column in that row group.
3. **Page**:
   - The smallest addressable unit of data within a column chunk (typically 1 MB) (see [[apache-parquet]]).
   - Pages are the boundaries where data encoding and compression are applied (see [[apache-parquet]]). Includes **Data Pages** (storing values) and **Dictionary Pages** (storing distinct values for dictionary encoding).
4. **File Footer**:
   - Stored at the physical end of the file.
   - Contains self-describing schema definitions, row group offsets, and column-level summary statistics (minimum and maximum values, null counts) (see [[apache-parquet]]).
   - **Reader Workflow**: Query engines read the file footer first to inspect metadata, locate exact byte offsets, and evaluate filters before scanning data pages (see [[apache-parquet]], [[query-io-optimizations]]).

## Encodings and Compression Codecs

Parquet maximizes storage efficiency by decoupling logical encoding from physical compression (see [[apache-parquet]]):

### Specialized Encodings
- **Dictionary Encoding**: Replaces repetitive string or integer values with compact integer dictionary keys (see [[apache-parquet]]).
- **Run-Length Encoding (RLE) & Bit-Packing**: Compresses consecutive identical values or low-bit integers into tightly packed bit streams (see [[apache-parquet]]).
- **Delta Encoding**: Stores differences between consecutive numbers or strings, ideal for sorted keys and timestamps (see [[apache-parquet]], [[query-io-optimizations]]).

### Columnar Compression Codecs
Compression is applied per page after encoding (see [[apache-parquet]]):
- **Snappy**: The default codec; prioritizes balanced, high-speed decompression with moderate compression ratios (see [[apache-parquet]]).
- **ZSTD (Zstandard)**: Offers high compression ratios with fast decompression speeds, making it increasingly popular for cold and warm analytical datasets (see [[apache-parquet]]).
- **Gzip & LZ4**: Supported for maximum archival density (Gzip) or ultra-fast CPU-bound pipelines (LZ4) (see [[apache-parquet]]).

## Nested Data Representation

Parquet natively supports rich, nested structures (such as `struct`, `list`, and `map`) without flattening schemas (see [[apache-parquet]]). It implements the Dremel algorithm using **definition levels** (tracking nullness at each nesting tier) and **repetition levels** (tracking where lists or repeated records reset) (see [[apache-parquet]]).

## Related

- [[apache-parquet]]
- [[query-io-optimizations]]
- [[apache-parquet-format]]
- [[parquet-small-files-problem]]
- [[spark-execution-model]]
- [[spark-partitioning-and-shuffle]]
