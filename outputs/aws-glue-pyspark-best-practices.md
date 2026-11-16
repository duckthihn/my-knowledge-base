# AWS Glue Jobs (PySpark): Best Practices for Writing & Cost Optimization

Checklist thực chiến khi xây dựng và vận hành **AWS Glue Jobs (PySpark)** trong dự án thực tế, tổng hợp từ knowledge base. Chi phí Glue được tính theo $\text{Cost} = \text{DPUs} \times \text{Runtime (hours)}$ (xem [[aws-glue-architecture-and-dpu]], [[aws-glue-job-optimization]]), do đó tối ưu hiệu năng Spark đồng nghĩa trực tiếp với tối ưu chi phí.

---

## 1. Tối ưu khi viết code PySpark

- **Áp dụng mô hình Hybrid (`DynamicFrame` → `DataFrame`)** (xem [[dynamicframe-vs-dataframe]]):
  - Dùng `DynamicFrame` + `resolveChoice()` ở bước ingest dữ liệu thô để xử lý schema drift và mixed types (`choice` type).
  - Gọi `.toDF()` chuyển sang **Spark DataFrame** cho toàn bộ các bước `filter`, `join`, `groupBy`, window functions để tận dụng **Catalyst Optimizer** và **Tungsten Engine**.
- **Tối ưu đọc dữ liệu với `push_down_predicate` và `groupFiles`** (xem [[glue-data-catalog]], [[aws-glue-job-optimization]]):
  - Dùng `push_down_predicate` khi đọc từ Glue Data Catalog để prune partition S3 ngay ở tầng metadata trước khi list/scan file.
  - Khi nguồn bị nhiều file nhỏ (Small Files Problem), cấu hình `additional_options={"groupFiles": "inPartition", "groupSize": "134217728"}` (128 MB) để gom file lúc đọc, tránh quá tải Driver (xem [[parquet-small-files-problem]]).
- **Tránh tạo Small Files khi ghi output** (xem [[aws-glue-job-optimization]], [[spark-partitioning-and-shuffle]], [[adaptive-query-execution]]):
  - Bật AQE (`spark.sql.adaptive.enabled = true`) để tự coalesce shuffle partitions nhỏ, hoặc chủ động `df.coalesce(n)` trước khi `.write.parquet()`.
- **Cài đặt Job Bookmarks đủ 3 thành phần cho incremental jobs** (xem [[aws-glue-job-optimization]]):
  - Bật bookmark trên job + truyền `transformation_ctx` riêng biệt cho từng source/sink + gọi `job.init()` ở đầu và `job.commit()` ở cuối script.
- **Gộp các narrow transformations vào cùng 1 job** (xem [[aws-glue-job-optimization]], [[spark-execution-model]]):
  - Không tách chuỗi `filter`, `select`, `withColumn` thành nhiều Glue Jobs trong Workflow vì mỗi job tốn 2–4 phút khởi động cluster và phát sinh I/O trung gian qua S3. Chỉ tách job khi khác hẳn nhu cầu compute (vd: `G.1X` vs `G.4X`) hoặc cần checkpoint dữ liệu rõ ràng.

---

## 2. Sizing, cấu hình & vận hành

- **Hiểu công thức concurrency và chọn Worker Type đúng triệu chứng** (xem [[aws-glue-architecture-and-dpu]]):
  - 1 worker dành cho Driver; số task chạy song song tối đa là $(\text{Total Workers} - 1) \times \text{vCPUs per Executor}$.
  - **Memory Spill / Executor OOM**: Scale **up** worker type (`G.1X` → `G.2X` 32 GB RAM hoặc `G.4X` 64 GB RAM) hoặc tăng số shuffle partitions.
  - **Nhiều task đồng đều, thiếu CPU**: Scale **out** tăng số worker.
  - **Data Skew (treo ở 99%)**: Không tăng worker; xử lý bằng AQE skew join (`spark.sql.adaptive.skewJoin.enabled = true`) hoặc salting key (xem [[adaptive-query-execution]], [[debugging-slow-spark-jobs]]).
- **Bật Auto-Scaling, Flex Execution & dùng Glue version mới** (xem [[aws-glue-architecture-and-dpu]], [[aws-glue-job-optimization]]):
  - Bật Auto-Scaling và kiểm tra CloudWatch metric `glue.driver.aggregate.numActiveExecutors` để hạ `MaxWorkers` nếu không dùng tới trần.
  - Dùng `ExecutionClass: FLEX` cho batch qua đêm, backfill, dev/test để giảm **30%–50%** chi phí DPU-hour.
  - Ưu tiên **Glue 4.0 / 5.0+** để hưởng Spark runtime mới hơn và đơn giá DPU tốt hơn.
- **Luôn đặt Job Timeout rõ ràng** (xem [[aws-glue-job-optimization]]):
  - Không để mặc định **48 giờ (2,880 phút)**; đặt timeout sát với thời gian chạy dự kiến (vd: `60` phút cho job chạy ~15 phút).
- **Quản lý Schema ở Production** (xem [[glue-data-catalog]], [[apache-iceberg-format]]):
  - Ưu tiên khai báo schema tường minh (DDL / IaC) hoặc dùng **Apache Iceberg** (Glue Catalog giữ atomic pointer) thay vì phụ thuộc Glue Crawler cho pipeline production.

---

## 3. Phạm vi wiki chưa đề cập

- Quy trình CI/CD, packaging thư viện (`.whl`/`.zip`), và unit testing PySpark/Glue local.
- Cấu hình VPC networking (subnet, NAT/S3 Gateway Endpoint, ENI) và IaC/alerting mẫu (Terraform, EventBridge, SNS).

## Sources Consulted

- [[aws-glue-job-optimization]]
- [[aws-glue-architecture-and-dpu]]
- [[dynamicframe-vs-dataframe]]
- [[glue-data-catalog]]
- [[adaptive-query-execution]]
- [[debugging-slow-spark-jobs]]
- [[parquet-small-files-problem]]
- [[spark-partitioning-and-shuffle]]
- [[spark-execution-model]]
- [[apache-iceberg-format]]
