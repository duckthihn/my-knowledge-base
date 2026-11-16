# Broadcast Join (Broadcast Hash Join) in Apache Spark: Concept & Mechanics

Tài liệu giải thích chi tiết về khái niệm, cơ chế hoạt động, cấu hình và so sánh của **Broadcast Join (Broadcast Hash Join)** trong Apache Spark, tổng hợp từ knowledge base.

---

## 1. Broadcast Join là gì?

**Broadcast Join** (hay `BroadcastHashJoin`) là một kỹ thuật tối ưu hóa phép join trong Apache Spark, áp dụng khi thực hiện join giữa một bảng rất lớn (Fact table) với một bảng nhỏ (Dimension table) (xem [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]]).

- **Mục đích chính**: Loại bỏ hoàn toàn giai đoạn **Network Shuffle Exchange** trên bảng lớn, tránh việc phải truyền dữ liệu qua mạng giữa các node trong cluster (xem [[spark-execution-model]], [[spark-partitioning-and-shuffle]]).

---

## 2. Cơ chế hoạt động (Step-by-Step)

Trong một phép Join thông thường (`SortMergeJoin`), Spark phải thực hiện **Shuffle Exchange**: băm (hash) key của cả 2 bảng và đẩy dữ liệu qua mạng để các bản ghi có cùng key tập trung về cùng một Executor (xem [[spark-partitioning-and-shuffle]]).

Đối với **Broadcast Join**, quy trình hoạt động như sau:

1. **Collect bảng nhỏ về Driver**: Driver process đọc/thu thập toàn bộ dữ liệu của bảng nhỏ (Dimension table) về bộ nhớ của Driver (xem [[spark-execution-model]]).
2. **Broadcast tới tất cả Executor**: Driver gửi một bản sao đầy đủ của bảng nhỏ này đến bộ nhớ (in-memory) của **tất cả các Executor** trong cluster (xem [[spark-partitioning-and-shuffle]]).
3. **Local Hash Join trên từng Executor**: 
   - Từng Executor lưu bản sao bảng nhỏ trong RAM và dựng một Hash Table trong bộ nhớ.
   - Các task trên Executor chỉ cần duyệt qua các partition local của bảng lớn và tra cứu trực tiếp vào Hash Table (xem [[spark-execution-model]], [[spark-partitioning-and-shuffle]]).
4. **Biến Wide Transformation thành Narrow Stage**: Bảng lớn giữ nguyên tại chỗ, không cần xáo trộn hay truyền qua mạng (bỏ qua bước `Exchange` trong DAG execution plan). Phép join trở thành một thao tác xử lý cục bộ giúp tốc độ thực thi nhanh hơn gấp nhiều lần (xem [[spark-execution-model]]).

---

## 3. Cách kích hoạt trong PySpark

- **Tự động kích hoạt (Threshold mặc định)**:
  - Spark tự động chuyển sang `BroadcastHashJoin` nếu dung lượng ước tính của bảng nhỏ dưới ngưỡng `spark.sql.autoBroadcastJoinThreshold` (mặc định là **10 MB**) (xem [[adaptive-query-execution]]).
- **Kích hoạt thủ công bằng hint `broadcast()`**:
  - Khi biết chắc một bảng kích thước nhỏ (thường < 10–50 MB), bạn có thể bọc bảng nhỏ bằng hàm `broadcast()` để bắt buộc Spark dùng Broadcast Join (xem [[debugging-slow-spark-jobs]]):
    ```python
    from pyspark.sql.functions import broadcast

    # Large fact table joined with small dimension table
    df_result = df_large_fact.join(broadcast(df_small_dim), "customer_id")
    ```
- **Tự động chuyển đổi runtime nhờ AQE**:
  - Khi bật **Adaptive Query Execution** (`spark.sql.adaptive.enabled = true`), nếu sau các bước lọc (`filter`) dữ liệu thực tế của một nhánh join giảm xuống dưới ngưỡng broadcast threshold, Spark AQE sẽ tự động đổi từ `SortMergeJoin` sang `BroadcastHashJoin` ngay tại runtime (xem [[adaptive-query-execution]]).

---

## 4. So sánh Broadcast Hash Join vs. SortMergeJoin

| Tiêu chí | Broadcast Hash Join | SortMergeJoin |
| :--- | :--- | :--- |
| **Kích thước bảng phù hợp** | 1 Bảng lớn + 1 Bảng nhỏ (< 10–50 MB) (xem [[debugging-slow-spark-jobs]]) | 2 Bảng đều lớn (xem [[debugging-slow-spark-jobs]]) |
| **Shuffle Exchange** | **Không có** (Skip Shuffle cho bảng lớn) (xem [[spark-partitioning-and-shuffle]]) | **Có** (Shuffle cả 2 bảng theo join key) (xem [[spark-partitioning-and-shuffle]]) |
| **Tải bộ nhớ (Memory)** | Cần RAM trên Driver & từng Executor đủ chứa bảng nhỏ (xem [[debugging-slow-spark-jobs]]) | Cần RAM/Disk cho Sort và Shuffle Spill (xem [[debugging-slow-spark-jobs]]) |
| **Nguy cơ lỗi OOM** | Tránh OOM do Shuffle Spill, nhưng có thể OOM Driver/Executor nếu bảng "nhỏ" bị quá to (xem [[debugging-slow-spark-jobs]]) | OOM Executor nếu partition bị lệch / Data Skew (xem [[debugging-slow-spark-jobs]]) |

---

## 5. Giới hạn & Lưu ý thực tế

- **Rủi ro Out-Of-Memory (OOM)**: Nếu lỡ bọc `broadcast()` cho một bảng có dung lượng lớn hơn bộ nhớ RAM sẵn có của Driver hoặc Executor, Driver sẽ bị đơ/crash do OOM khi cố thu thập toàn bộ bảng về.
- **Không áp dụng khi cả 2 bảng đều to**: Khi cả 2 bảng đều có dung lượng hàng GB/TB, việc broadcast sẽ làm quá tải RAM và mạng của cluster; lúc này bắt buộc phải dùng `SortMergeJoin` kết hợp tối ưu partition hoặc key salting (xem [[debugging-slow-spark-jobs]]).

---

## 6. Phạm vi wiki chưa đề cập

- Cấu trúc dữ liệu nội bộ (`HashedRelation`, `LongHashedRelation`) ở tầng Java/Scala engine.
- Cấu hình Timeout cho quá trình broadcast (`spark.sql.broadcastTimeout`, mặc định 300s).

## Sources Consulted

- [[spark-partitioning-and-shuffle]]
- [[spark-execution-model]]
- [[debugging-slow-spark-jobs]]
- [[adaptive-query-execution]]
- [[learn-spark]]
- [[apache-spark]]
