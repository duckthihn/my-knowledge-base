# Coalesce in Apache Spark: Concept, Use Cases & Best Practices

Tài liệu giải thích chi tiết về khái niệm, cơ chế hoạt động, tác dụng và lưu ý khi sử dụng **`coalesce`** trong Apache Spark, tổng hợp từ knowledge base.

---

## 1. Khái niệm & Cơ chế hoạt động của `coalesce`

Trong Apache Spark, **`coalesce(num_partitions)`** là phương thức dùng để **giảm số lượng partition** của DataFrame/RDD mà **không làm phát sinh full network shuffle** (xem [[spark-partitioning-and-shuffle]], [[learn-spark]]).

- **Gom partition tại chỗ (Local Merge)**: `coalesce` ghép các partition kề nhau trên cùng node/executor lại với nhau thay vì phân phối lại dữ liệu qua mạng (xem [[spark-partitioning-and-shuffle]]).
- **Bỏ qua Network Shuffle**: Khác với `repartition()` (vốn thực hiện wide transformation và shuffle toàn bộ dữ liệu qua mạng), `coalesce` tận dụng tính cục bộ của dữ liệu (data locality), giúp thao tác diễn ra cực kỳ nhanh và ít tốn tài nguyên (xem [[spark-partitioning-and-shuffle]], [[spark-execution-model]]).

---

## 2. Các tác dụng chính của `coalesce`

### ① Ngăn chặn và giải quyết bài toán Small Files (Tác dụng quan trọng nhất)
- **Vấn đề**: Sau khi thực hiện các wide transformation (`groupBy`, `join`), Spark mặc định chia dữ liệu ra `spark.sql.shuffle.partitions = 200` partition. Nếu bạn lọc (`filter`) dữ liệu chỉ còn 10 MB rồi ghi thẳng ra Parquet/S3, Spark sẽ tạo ra 200 file Parquet siêu nhỏ (~50 KB/file), gây ra bài toán **Parquet Small Files Problem** (xem [[parquet-small-files-problem]], [[aws-glue-job-optimization]]).
- **Giải pháp**: Gọi `df.coalesce(1)` hoặc `df.coalesce(optimal_n)` ngay trước bước ghi `.write.parquet()` để gom dữ liệu thành các file có kích thước chuẩn (128 MB – 256 MB) (xem [[parquet-small-files-problem]]):
  ```python
  # Gộp partition trước khi ghi ra S3 để tránh đẻ file nhỏ
  df.filter("date = '2026-11-13'") \
    .coalesce(2) \
    .write.parquet("s3://bucket/curated/")
  ```

### ② Tiết kiệm chi phí Network I/O và Disk Serialization
- Vì không đẩy dữ liệu qua mạng giữa các node, `coalesce` giảm tải hoàn toàn CPU serialization, băng thông mạng và Disk I/O spill so với `repartition` (xem [[spark-partitioning-and-shuffle]]).

### ③ Giảm tải Driver Task Scheduling Overhead
- Sau các bước lọc loại bỏ 90-95% dữ liệu, nếu giữ nguyên số partition cũ, Driver sẽ phải mất thời gian khởi tạo và quản lý hàng ngàn task rỗng/siêu nhỏ cho các stage tiếp theo. `coalesce` thu gọn lại số task giúp Driver làm việc hiệu quả hơn (xem [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]]).

---

## 3. So sánh `coalesce()` vs. `repartition()`

| Tiêu chí | `coalesce(n)` | `repartition(n)` |
| :--- | :--- | :--- |
| **Mục đích chính** | Chiều **giảm** số partition (xem [[spark-partitioning-and-shuffle]]) | Tăng hoặc giảm số partition, chia lại dữ liệu theo key (xem [[spark-partitioning-and-shuffle]]) |
| **Network Shuffle** | **Không** (Dùng local merge) (xem [[spark-partitioning-and-shuffle]]) | **Có** (Full cluster shuffle qua mạng) (xem [[spark-partitioning-and-shuffle]]) |
| **Chi phí tính toán** | Rất nhẹ và nhanh (xem [[spark-partitioning-and-shuffle]]) | Đắt (tốn CPU, RAM, Network & Disk I/O) (xem [[spark-partitioning-and-shuffle]]) |
| **Cân bằng dữ liệu** | Dữ liệu giữa các partition gộp có thể không hoàn toàn bằng nhau | Dữ liệu được băm (hash) phân phối rất đều giữa các partition |
| **Use case điển hình** | Gom file nhỏ trước khi ghi Parquet/S3, thu gọn partition sau `filter` | Tăng số partition khi thiếu RAM/OOM, phân phối lại dữ liệu bị skew |

---

## 4. Tự động Coalesce nhờ Adaptive Query Execution (AQE)

Từ Spark 3.0+ (Glue 3.0+), bạn không bắt buộc phải gọi `coalesce()` thủ công cho các bước shuffle nhờ tính năng **AQE Dynamic Shuffle Partition Coalescing** (`spark.sql.adaptive.enabled = true`) (xem [[adaptive-query-execution]], [[aws-glue-job-optimization]]):
- Sau mỗi shuffle stage, AQE tự động kiểm tra kích thước thực tế của các shuffle partition và tự gom (coalesce) các partition nhỏ liên tiếp thành 1 partition lớn hơn trước khi chuyển sang stage tiếp theo (xem [[adaptive-query-execution]]).

---

## 5. Cạm bẫy cần tránh khi dùng `coalesce`

1. **Không thể dùng để TĂNG số partition**: Nếu truyền `n > số partition hiện tại`, `coalesce` sẽ bị bỏ qua hoặc không có hiệu lực. Để tăng partition nhằm tăng mức độ xử lý song song, bắt buộc phải dùng `repartition()` (xem [[spark-partitioning-and-shuffle]]).
2. **Ép về `coalesce(1)` quá sớm**: Nếu gọi `df.coalesce(1)` trước các bước tính toán nặng (`map`, `filter`, `withColumn`), Spark sẽ co toàn bộ execution stream của stage đó về chạy trên 1 task/1 core duy nhất, làm mất tính toán song song và dễ gây lỗi Out-Of-Memory (`Executor OOM`) (xem [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]]).

---

## 6. Phạm vi wiki chưa đề cập

- Hàm `coalesce(col1, col2, ...)` trong Spark SQL (hàm trả về giá trị non-null đầu tiên trong danh sách cột, phân biệt với phương thức `df.coalesce(n)` điều chỉnh partition của DataFrame).

## Sources Consulted

- [[spark-partitioning-and-shuffle]]
- [[parquet-small-files-problem]]
- [[adaptive-query-execution]]
- [[aws-glue-job-optimization]]
- [[debugging-slow-spark-jobs]]
- [[spark-execution-model]]
- [[learn-spark]]
