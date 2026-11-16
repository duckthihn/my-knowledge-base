# Delta Lake vs Apache Iceberg: So sánh cơ chế ACID, Kiến trúc & Use Cases

**Date**: 2026-11-13  
**Sources**: [[delta-lake]], [[apache-iceberg-format]], [[lakehouse-architecture]], [[iceberg-metadata-architecture]], [[iceberg-table-optimizations]], [[iceberg-table-maintenance]]

---

Tài liệu so sánh chuyên sâu giữa hai định dạng table format phổ biến trong kiến trúc Data Lakehouse: **Delta Lake** và **Apache Iceberg**, tổng hợp từ knowledge base.

---

## 1. Cơ chế ACID — Điểm khác biệt cốt lõi

Cả hai đều đảm bảo ACID trên object storage, nhưng bằng cơ chế hoàn toàn khác nhau (xem [[delta-lake]], [[iceberg-metadata-architecture]]):

### 1.1. Delta Lake — Transaction Log (`_delta_log`)

```
s3://bucket/my-table/
├── _delta_log/
│   ├── 00000000000000000000.json   ← commit 0: INSERT
│   ├── 00000000000000000001.json   ← commit 1: UPDATE
│   ├── 00000000000000000002.json   ← commit 2: DELETE
│   └── 00000000000000000010.checkpoint.parquet
├── part-0001.parquet
└── part-0002.parquet
```

- Mỗi write **append** một JSON commit entry vào `_delta_log/`
- Reader **replay** log từ checkpoint mới nhất để tái tạo table state
- **Không cần catalog** cho atomic commit — log file trên S3 IS the source of truth
- Checkpoint định kỳ (mỗi 10 commits) để tránh replay quá dài

### 1.2. Apache Iceberg — Catalog Pointer Swap (CAS)

```
Catalog (AWS Glue)
  └── pointer: s3://.../metadata/v3.metadata.json   ← atomic swap

s3://bucket/my-table/metadata/
├── v1.metadata.json
├── v2.metadata.json
├── v3.metadata.json  ← current
├── snap-101.avro     ← manifest list
└── manifest-a.avro   ← per-file stats

s3://bucket/my-table/data/
├── file-1.parquet
└── file-2.parquet
```

- Write tạo files mới → tạo metadata mới → **atomic CAS** pointer trong Catalog
- Reader chỉ cần hỏi Catalog → lấy metadata file → biết ngay toàn bộ table state
- **Catalog là bắt buộc** (AWS Glue, Nessie, REST) để hỗ trợ CAS
- Không cần `LIST` S3 bao giờ

---

## 2. Bảng so sánh toàn diện

| Tiêu chí | Delta Lake | Apache Iceberg |
|---|---|---|
| **Cơ chế ACID** | `_delta_log` JSON/Parquet entries | Catalog atomic pointer swap (OCC) |
| **Catalog bắt buộc?** | ❌ Không (log trên S3 là đủ) | ✅ Có (Glue, Nessie, REST...) |
| **Engine independence** | Cao (Spark, Trino, Flink; tối ưu cho Databricks) | **Xuất sắc** (broadest vendor support) |
| **Partition Evolution** | ❌ Hạn chế (đổi strategy → phải rewrite data) | ✅ **Native**, zero data rewrites |
| **Hidden Partitioning** | ❌ Không (phải query đúng partition column) | ✅ Có (`days(ts)`, `bucket(16, id)`) |
| **Schema Evolution** | ✅ Có (`MERGE SCHEMA`) | ✅ Có (column ID-based, không rewrite) |
| **Time Travel** | ✅ `VERSION AS OF n` / `TIMESTAMP AS OF` | ✅ `TIMESTAMP AS OF` / `BRANCH` |
| **COW / MOR** | COW mặc định | Cả hai, chọn per-table |
| **Deletion Vectors** | ✅ Roaring bitmaps | ✅ Format v3 (Iceberg 1.11.0+) — cùng binary spec |
| **Compaction** | `OPTIMIZE` + `Z-ORDER BY` | `rewrite_data_files` (bin-packing, sorting) |
| **File discovery** | Replay `_delta_log` | Đọc manifest files (không LIST S3) |
| **Multi-engine** | Spark, Trino, Flink, DuckDB... | Spark, Trino, Flink, Athena, Snowflake, BigQuery... |
| **Primary ecosystem** | **Databricks Lakehouse** | **Vendor-neutral open standard** |
| **Streaming / CDC** | Tốt | Tốt (MOR + v3 Deletion Vectors) |

---

## 3. Xu hướng Format Convergence

Databricks encode Delta Lake deletion vectors **bằng đúng binary specification** của Iceberg Format v3 Deletion Vectors, cho phép bidirectional interoperability giữa hai format mà không cần copy data (xem [[apache-iceberg-format]], [[delta-lake]]).

---

## 4. Hướng dẫn lựa chọn (Khi nào dùng cái nào?)

| Chọn **Delta Lake** khi... | Chọn **Apache Iceberg** khi... |
|---|---|
| Team đang dùng **Databricks** — tích hợp sâu nhất, ít friction nhất | Cần **multi-engine** thực sự (Spark + Athena + Flink + Snowflake + BigQuery) |
| Workload chủ yếu là **Spark-only** | Cần **partition evolution** — đổi strategy mà không migrate data |
| Muốn `OPTIMIZE` + **Z-ORDER** clustering đơn giản | Cần **hidden partitioning** — user query cột gốc, không cần biết partition scheme |
| Không muốn phụ thuộc vào catalog bên ngoài | Đang dùng **AWS native stack** (Glue Catalog, Athena, Lake Formation) |
| Ecosystem chủ yếu trong **AWS + Databricks** | Cần **vendor-neutral open standard** cho long-term flexibility |

---

## 5. Phạm vi wiki chưa đề cập

- Chi tiết UniForm (Universal Format) của Databricks giúp đọc Delta tables dưới dạng Iceberg reader.
- So sánh chi tiết hiệu năng và cơ chế lưu trữ của Apache Hudi so với Delta Lake và Iceberg.

## Sources Consulted

- [[delta-lake]]
- [[apache-iceberg-format]]
- [[lakehouse-architecture]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[iceberg-table-maintenance]]
