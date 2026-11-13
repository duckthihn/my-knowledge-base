---
type: source
title: Apache Iceberg - Open Table Format
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [iceberg, table-format, data-lake, acid, metadata, optimization]
status: stable
---

# Apache Iceberg: Architecture, Operations, and Maintenance

- **Original Title**: Iceberg Fundamentals / Architecture & AWS Glue Integration
- **Author / Source**: [Internal Notes & AWS Documentation](raw/apache-iceberg.md)
- **Path in raw**: `raw/apache-iceberg.md`

## Summary

A comprehensive operational and architectural guide to Apache Iceberg, an open table format that places a hierarchical metadata layer over columnar data files (Parquet, ORC, Avro). The source details the limitations of raw Hive-style data lakes, the 3-tier metadata architecture (Catalog, Metadata Files, Manifest Lists, Manifest Files), atomic ACID commit protocols, two-tier pruning, hidden partitioning, schema evolution, and row-level operations (Copy-On-Write vs. Merge-On-Read). It also explores Format v3 deletion vectors, AWS Glue / PySpark configurations, and essential maintenance procedures.

## Key Takeaways

- **Solving Raw Data Lake Bottlenecks**: Traditional Hive-style S3 partitioning (`year=/month=`) lacks ACID guarantees, requires expensive filesystem `LIST` calls across millions of objects, makes schema changes brittle, and locks tables into rigid directory layouts. Iceberg defines tables strictly through metadata files, eliminating directory scanning entirely.
- **Hierarchical Metadata Architecture**: Organizes tables into a 3-tier tree:
  1. *Catalog*: Stores the atomic pointer to the current metadata file.
  2. *Metadata Layer*: Composed of Metadata Files (table schema, partition specs, snapshots), Manifest Lists (snapshot state pointing to manifests), and Manifest Files (tracks individual data/delete files with column min/max metrics).
  3. *Data Layer*: Physical data files (Parquet) and delete files.
- **Two-Tier Query Pruning**: Query engines evaluate min/max column metrics within Manifest Files to prune entire data files before opening them, which directly chains into Parquet internal footer min/max evaluation to prune row groups.
- **Schema & Partition Evolution**: Columns are tracked by persistent integer IDs rather than names or physical positions, allowing columns to be added, renamed, dropped, or reordered without data rewrites. Hidden partitioning applies logical transforms (`days(ts)`), decoupling user query predicates from physical storage and allowing partition strategies to evolve over time without data migration.
- **Row-Level Mutations & Format v3 Deletion Vectors**: `MERGE INTO` operations execute as wide transformation shuffle joins in Spark. While Copy-On-Write (COW) incurs write amplification and Merge-On-Read (MOR) degrades scan performance due to accumulating delete files, Iceberg Format v3 introduces **Deletion Vectors** (compact compressed bitmaps, production-ready since Iceberg 1.11.0 / May 2026) to deliver fast writes without compromising read speed.
- **Mandatory Table Maintenance**: Iceberg does not self-compact. Workloads require periodic Spark procedures: `rewrite_data_files` (combining small files into optimal 128–512 MB files), `expire_snapshots` (pruning historical commits to reclaim S3 storage), and `remove_orphan_files`.

## Notable Quotes

- *"Bảng được định nghĩa bởi danh sách file trong metadata, không phải bởi nội dung thư mục... Engine không cần LIST S3, chỉ đọc metadata là biết chính xác file nào thuộc bảng."* (Context: Explaining the foundational paradigm shift from Hive-style lakes to Iceberg).
- *"Iceberg catalog is required to support atomic operations when updating the pointer, which ensures that all readers and writers see the same table state at any moment."* (Context: Explaining ACID transaction mechanics via atomic pointer swaps).
- *"Điểm đáng chú ý nhất của v3 là deletion vectors. Chúng thay các positional delete file của v2 bằng các bitmap nén gọn, giúp đọc ghi nhanh hơn với workload update và xóa tần suất cao."* (Context: Highlighting the major performance unlock of Iceberg v3).
- *"MERGE INTO thực chất là join theo key, nên gây shuffle... Key lệch làm MERGE chậm, y như join thông thường."* (Context: Connecting table format operations directly to Spark Day 1 execution and skew concepts).

## Pages Created or Updated

- [[iceberg-metadata-architecture]]: 3-tier metadata tree, Catalog pointer mechanics, snapshot isolation, and atomic commit workflows.
- [[iceberg-table-optimizations]]: Two-tier pruning, hidden partitioning, schema evolution via column IDs, COW vs. MOR, and Format v3 Deletion Vectors.
- [[apache-iceberg-format]]: Entity overview, comparison with Delta Lake and Apache Hudi, multi-engine interoperability, and AWS Glue / PySpark integration.
- [[iceberg-table-maintenance]]: Maintenance procedures (`rewrite_data_files`, `expire_snapshots`, `remove_orphan_files`), delete file bloat, and operational pitfalls.
- [[query-io-optimizations]]: Updated to cover Iceberg manifest-level file pruning alongside Parquet row-group pushdown.
- [[parquet-small-files-problem]]: Updated with Iceberg `rewrite_data_files` bin-packing compaction procedures.
- [[debugging-slow-spark-jobs]]: Updated with troubleshooting guidelines for `MERGE INTO` shuffle joins, data skew, and un-compacted MOR tables.

## Related

- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[apache-iceberg-format]]
- [[iceberg-table-maintenance]]
- [[query-io-optimizations]]
- [[parquet-small-files-problem]]
- [[debugging-slow-spark-jobs]]
- [[spark-execution-model]]
