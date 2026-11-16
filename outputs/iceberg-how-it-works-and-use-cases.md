# Apache Iceberg: Concept, Mechanics & Use Cases

**Date**: 2026-11-13  
**Sources**: [[iceberg-metadata-architecture]], [[iceberg-table-optimizations]], [[apache-iceberg-format]], [[apache-iceberg]], [[iceberg-table-maintenance]]

---

Tài liệu giải thích chi tiết về cơ chế hoạt động, kiến trúc 3 tầng và các kịch bản sử dụng (use cases) của **Apache Iceberg**, tổng hợp từ knowledge base.

---

## 1. Kiến trúc & Cơ chế hoạt động của Apache Iceberg

### 1.1. Kiến trúc 3 tầng (3-Tier Metadata Hierarchy)

Iceberg không định nghĩa bảng bằng thư mục vật lý như Hive-style (e.g. `s3://bucket/year=2026/month=11/`). Thay vào đó, **bảng được định nghĩa bởi metadata files** — engine không cần `LIST` S3, chỉ cần đọc metadata là biết chính xác file nào thuộc bảng (xem [[iceberg-metadata-architecture]]).

```
┌─────────────────────────────────────────────────────┐
│ 1. CATALOG LAYER (AWS Glue, Nessie, Hive Metastore) │
│    → Lưu pointer đến current metadata file          │
│      e.g.  s3://.../v3.metadata.json                │
└──────────────────────┬──────────────────────────────┘
                       │ trỏ tới
                       ▼
┌─────────────────────────────────────────────────────┐
│ 2. METADATA LAYER                                   │
│    a. Metadata Files (*.metadata.json)              │
│       - schema, partition spec, snapshot log        │
│    b. Manifest List (snap-*.avro)                   │
│       - danh sách manifest files của 1 snapshot     │
│    c. Manifest Files (*.avro)                       │
│       - danh sách data files + col min/max stats    │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│ 3. DATA LAYER                                       │
│    Data Files (.parquet) + Delete Files             │
└─────────────────────────────────────────────────────┘
```

### 1.2. Read Path & Write Path

**Read (không có một `LIST` S3 nào):**
1. Engine hỏi Catalog → lấy current metadata file
2. Đọc metadata file → xác định snapshot hiện tại → mở Manifest List
3. **Tier 1 Pruning**: Đọc Manifest Files, dùng `[min, max]` stats từng file → loại file không khớp predicate
4. **Tier 2 Pruning**: Mở từng Parquet file sót lại → Parquet footer min/max → skip Row Groups

```
Query: SELECT * WHERE amount > 5000 AND order_ts >= '2026-11-01'

[Tier 1 - Manifest Pruning]
  10,000 Data Files → 40 candidate files  (0 bytes đọc từ data)

[Tier 2 - Parquet Row Group Pushdown]
  40 files → chỉ scan Row Groups khớp predicate
```

**Write (ACID qua Optimistic Concurrency Control):**
1. Writer ghi Parquet files mới ra S3 (immutable, không sửa file cũ)
2. Tạo manifest files mới + metadata file mới
3. **Atomic CAS**: Catalog swap pointer từ `v2.json` → `v3.json`
4. Nếu writer khác commit trước → check conflict → rebase/retry hoặc fail safe

### 1.3. Tính năng cốt lõi

| Tính năng | Mô tả |
|---|---|
| **Hidden Partitioning** | Dùng `days(order_ts)`, `bucket(16, user_id)` — user query cột gốc, Iceberg tự prune partition |
| **Partition Evolution** | Đổi partition strategy **không cần rewrite data cũ** |
| **Schema Evolution** | Columns theo dõi bằng integer ID — rename/drop/reorder không rewrite file |
| **Time Travel** | Query `TIMESTAMP AS OF '2026-11-01'` trả về đúng snapshot lịch sử |
| **COW vs MOR** | Copy-On-Write: read nhanh, write chậm; Merge-On-Read: write nhanh, cần compact định kỳ |
| **Format v3 Deletion Vectors** | Roaring bitmap thay positional delete files — xóa/update nhanh mà không hại read |

---

## 2. Khi nào nên dùng Iceberg?

### 2.1. Dùng Iceberg khi:

| Scenario | Lý do |
|---|---|
| **Multi-engine data lake** (Spark + Athena + Flink + Trino) | Iceberg engine-neutral, không lock-in vendor |
| **ACID trên object storage** (S3, GCS) | Snapshot isolation + atomic CAS — không lo partial write |
| **CDC / MERGE INTO từ OLTP** | MOR mode + Format v3 Deletion Vectors |
| **Bảng hàng triệu files, query slow** | 2-tier pruning + hidden partitioning |
| **Schema thay đổi thường xuyên** | Column ID-based evolution |
| **Partition strategy cần thay đổi** | Partition evolution in-place |
| **Audit / data lineage / rollback** | Time travel qua snapshot history |
| **Replace Hive-style S3 lake** | Không còn `LIST` S3 tốn kém |

### 2.2. Không cần Iceberg khi:

| Scenario | Giải thích |
|---|---|
| **Chỉ dùng Databricks** | Delta Lake tích hợp sâu hơn trong ecosystem đó |
| **Near-real-time streaming CDC đơn thuần** | Apache Hudi tối ưu hơn |
| **Bảng nhỏ, query đơn giản, không cần ACID** | Plain Parquet đủ dùng |
| **Team chưa sẵn sàng vận hành** | Iceberg không tự compact — cần schedule maintenance jobs |

### 2.3. Lưu ý vận hành

Iceberg không tự dọn dẹp. Phải schedule 3 jobs định kỳ (xem [[iceberg-table-maintenance]]):
- `rewrite_data_files` — compact small files → 128–512 MB
- `expire_snapshots` — xóa old snapshots, thu hồi S3 storage
- `remove_orphan_files` — xóa files từ failed jobs chưa được commit

---

## 3. Phạm vi wiki chưa đề cập

- Chi tiết chuẩn REST Catalog OpenAPI spec và triển khai Nessie / Polaris Catalog cho production.
- Quy trình Write-Audit-Publish (WAP) pattern với Iceberg branches.

## Sources Consulted

- [[apache-iceberg]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[apache-iceberg-format]]
- [[iceberg-table-maintenance]]
