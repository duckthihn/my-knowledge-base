---
type: source
title: Apache Parquet - Columnar Storage Format
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [parquet, columnar, storage, analytics, optimization]
status: stable
---

# Apache Parquet: Everything You Need to Know

- **Original Title**: What is Parquet? / Here's Everything You Need to Know
- **Author / Source**: [Vu Tr. (Substack)](https://vutr.substack.com/p/heres-everything-you-need-to-know)
- **Path in raw**: `raw/apache-parquet.md`

## Summary

An overview of Apache Parquet, an open-source columnar storage format optimized for big data analytics. The article details the columnar layout, file anatomy (row groups, column chunks, pages, footer), encodings, compression algorithms, and query optimization features like column pruning and predicate pushdown. It also contrasts Parquet with row-based formats (CSV, Avro), evaluates its OLAP sweet spot against its immutability trade-offs, and provides practical sizing and sorting guidelines.

## Key Takeaways

- **Columnar vs. Row-based Mechanics**: Parquet stores values contiguously by column rather than row. This enables column pruning (reading only referenced fields) and high compression ratios due to homogeneous data types, dramatically decreasing analytical I/O and enabling vectorized query execution.
- **Hierarchical File Structure**: Files consist of Row Groups (parallelism units, tens to hundreds of MB), Column Chunks (per-column data within a row group), and Pages (smallest encoding/compression units). A trailing Footer stores schemas, offsets, and column min/max statistics.
- **Predicate Pushdown & Encodings**: Query engines read footer min/max metadata first to prune entire row groups without touching data pages. Parquet applies intelligent encodings (dictionary, RLE, bit-packing, delta) and per-column compression (Snappy, ZSTD, Gzip).
- **Immutability & Table Format Evolution**: Parquet files are immutable, making row-level OLTP updates and high-frequency streaming writes inefficient. Modern open table formats (Delta Lake, Apache Iceberg, Apache Hudi) add ACID transactions, time travel, and upsert/delete capabilities on top of Parquet.
- **Practical Storage Optimizations**: Target file sizes of 128 MB to 1 GB (aligning with Spark partition split sizes). Sort datasets on frequently filtered columns to cluster values and tighten min/max ranges, and mitigate the small files problem to avoid metastore and query engine throttling.

## Notable Quotes

- *"Chỉ đọc cột cần thiết. Truy vấn SELECT AVG(tuổi) chỉ đọc cột tuổi, bỏ qua phần còn lại (column pruning). Với bảng hàng trăm cột thì tiết kiệm I/O rất lớn."* (Context: Explaining the primary I/O advantage of columnar storage).
- *"Footer: chứa schema và thống kê, nằm ở cuối file. Reader đọc footer trước để biết cần đọc phần nào."* (Context: Explaining how engines leverage footer metadata to avoid unneeded scans).
- *"File Parquet là bất biến (immutable), muốn sửa phải ghi lại file... Để khắc phục hạn chế cập nhật, người ta xây các table format như Delta Lake, Apache Iceberg, Apache Hudi."* (Context: Describing why lakehouse table formats emerged).
- *"Sắp xếp dữ liệu theo cột hay lọc để thống kê min/max hiệu quả hơn."* (Context: Recommending data sorting to maximize predicate pushdown efficiency).

## Pages Created or Updated

- [[parquet-file-architecture]]: Internal layout of Row Groups, Column Chunks, Pages, Footer metadata, encodings, and compression.
- [[query-io-optimizations]]: Column pruning, predicate pushdown mechanics, data sorting for min/max statistics tightening, and file sizing.
- [[apache-parquet-format]]: History (Twitter, Cloudera, Google Dremel), engine interoperability, and relationship to Delta Lake/Iceberg/Hudi.
- [[parquet-small-files-problem]]: Root causes, object storage and compute performance impacts, and compaction mitigation strategies.
- [[spark-partitioning-and-shuffle]]: Updated with links between Parquet file sizing and Spark input partition splits.
- [[debugging-slow-spark-jobs]]: Updated with links connecting PySpark early filtering/projection to Parquet pushdown and column pruning.

## Related

- [[parquet-file-architecture]]
- [[query-io-optimizations]]
- [[apache-parquet-format]]
- [[parquet-small-files-problem]]
- [[spark-partitioning-and-shuffle]]
- [[debugging-slow-spark-jobs]]
