## 1. Vấn đề của data lake "thô"

![[Pasted image 20261113165327.png]]

Khi lưu Parquet trên S3 theo kiểu thư mục `year=/month=` và dùng Hive-style Catalog, bạn gặp các giới hạn sau:

|Vấn đề|Nguyên nhân|
|---|---|
|Không có ACID|Ghi 100 file mà job chết giữa chừng thì reader thấy dữ liệu dở dang|
|Update/delete rất khó|Object storage là immutable, muốn sửa 1 dòng phải ghi lại cả file|
|Liệt kê file chậm|Muốn biết bảng có file nào phải `LIST` S3, chậm khi có hàng triệu file|
|Đổi schema dễ vỡ|Đổi tên cột hay đổi kiểu có thể làm hỏng dữ liệu cũ|
|Đổi cách partition = viết lại bảng|Partition gắn cứng vào cấu trúc thư mục|
|Đọc và ghi đồng thời xung đột|Không có cơ chế cô lập|

## 2. Iceberg là gì

Iceberg là một **open table format**: một lớp **metadata** đặt lên trên các file dữ liệu (Parquet, ORC, Avro), cho phép coi tập file đó như một bảng có giao dịch, lịch sử và schema rõ ràng.

Cần phân biệt ba khái niệm hay bị nhầm:

|Lớp|Là gì|Ví dụ|
|---|---|---|
|**File format**|Cách lưu dữ liệu trong 1 file|Parquet, ORC|
|**Table format**|Cách tổ chức nhiều file thành 1 bảng, có metadata|**Iceberg**, Delta Lake, Hudi|
|**Catalog**|Nơi lưu "con trỏ" tới metadata hiện tại của bảng|Glue Data Catalog, Hive Metastore|

## 3. Architecture

![image.png](attachment:36956a93-3289-419b-abc0-9ac51b39779a:image.png)

[iceberg_metadata_layers_walkthrough.html](attachment:d507ebb3-83f0-4d44-83f6-cb1aba9a1354:iceberg_metadata_layers_walkthrough.html)

Bảng được định nghĩa bởi **danh sách file trong metadata**, không phải bởi nội dung thư mục. Hệ quả:

- Engine không cần `LIST` S3, chỉ đọc metadata là biết chính xác file nào thuộc bảng.
    
- Manifest chứa **thống kê min/max** theo cột nên engine bỏ qua được các file không liên quan (file pruning), mạnh hơn partition pruning thông thường.
    
- Mỗi lần ghi tạo ra một **snapshot** mới, các snapshot cũ vẫn còn nên có time travel.
    
- Architecture
    
    An Apache Iceberg table has three layers organized hierarchically: the catalog layer is at the top, followed by the metadata layer, which includes metadata files, the manifest list, and the manifests file. The final layer is the data. Since each higher layer tracks the information of the one below it, we'll start from the bottom and work our way up.
    
    # **The data layer**
    
    This layer stores the table’s actual data, including the data and deleted files. Data files store the data itself, and delete files keep track of which records in the table have been deleted. Delete files in Iceberg using the merge-on-read strategy. It will create a new file that records the deletion; the reader must merge the deleted files with the data files to retrieve the final result.
    
    # **Manifest Files**
    
    Manifest files keep track of the data files in the data layer and provide statistics, like the minimum and maximum values for each column in a data file. They also define the file format of the data files, such as Parquet, ORC, or Avro.
    
    # **Manifest Lists**
    
    A manifest list captures the snapshot of an Iceberg table at a specific moment. It includes a list of manifest files and details such as the manifest file’s location and partition information. Essentially, a manifest list is an array of structs, each representing a single manifest file.
    
    # **Metadata Files**
    
    Metadata files contain information about an Iceberg table at a specific time, such as the table's schema, partition details, snapshots, and the latest snapshot. When the Iceberg table changes, the catalog creates a new metadata file and marks it as the latest version. This process maintains a linear history of table commits. Additionally, Iceberg always presents the table's latest version to the reader.
    
    # **The catalog**
    
    The catalog is where every Iceberg data operation begins. It guides you on where to go first. The catalog will point you to the current metadata pointer's location. Iceberg catalog is required to support atomic operations when updating the pointer, which ensures that all readers and writers see the same table state at any moment.
    
    Many different backends can serve as catalogs for Iceberg, such as Hive Metastore, Nessie, or AWS Glue Catalog.
    

## 4. Đọc và ghi hoạt động ra sao

**Đọc:**

```
1. Hỏi catalog: metadata file hiện tại của bảng?
2. Đọc metadata → lấy snapshot hiện tại → manifest list
3. Đọc manifest, dùng thống kê min/max để loại file không cần
4. Chỉ đọc các data file còn lại
```

**Ghi:**

```
1. Ghi data file mới lên S3 (file cũ không bị sửa)
2. Tạo manifest và metadata file mới mô tả trạng thái sau khi ghi
3. Cập nhật con trỏ trong catalog sang metadata mới (một bước atomic)
```

Bước 3 là chỗ tạo ra **ACID**: reader hoặc thấy trạng thái cũ, hoặc thấy trạng thái mới, không bao giờ thấy trạng thái dở. Khi hai writer ghi đồng thời, Iceberg dùng **optimistic concurrency**: ai commit trước thắng, người sau kiểm tra xung đột và thử lại hoặc báo lỗi.

## 5. Các tính năng chính

**Schema evolution an toàn.** Iceberg theo dõi cột bằng **ID** chứ không bằng tên hay vị trí, nên thêm, xóa, đổi tên, sắp xếp lại cột không cần viết lại dữ liệu và không làm hỏng dữ liệu cũ.

**Hidden partitioning và partition evolution.** Bạn khai báo partition bằng biến đổi, ví dụ `days(order_ts)`. Người query chỉ lọc theo `order_ts`, Iceberg tự biết partition nào cần đọc. Bạn cũng có thể **đổi cách partition** sau này (từ theo tháng sang theo ngày) mà không viết lại dữ liệu cũ.

**Time travel và rollback.** Truy vấn bảng tại một thời điểm hoặc snapshot trong quá khứ, hoặc khôi phục khi ghi nhầm.

**Row-level operations.** `UPDATE`, `DELETE`, `MERGE INTO` có hai cách thực hiện:

||Copy-on-write|Merge-on-read|
|---|---|---|
|Khi ghi|Viết lại các file bị ảnh hưởng|Chỉ ghi file "xóa" nhỏ, giữ file gốc|
|Ghi|Chậm hơn|Nhanh hơn|
|Đọc|Nhanh|Phải áp dụng delete lên khi đọc|
|Hợp với|Ít update, đọc nhiều|Update thường xuyên, CDC|

**Table maintenance.** Iceberg không tự dọn dẹp, bạn cần chạy định kỳ: gộp file nhỏ (compaction), xóa snapshot cũ, xóa file mồ côi.

## 6. Các phiên bản format (v1, v2, v3)

- **v1:** bảng analytic cơ bản. **v2:** thêm row-level delete bằng _delete file_.
- **v3** là bản mới nhất. Theo tài liệu AWS, v3 nhằm giải quyết các nút thắt hiệu năng của v2, đặc biệt ở cập nhật theo lô và xóa phục vụ tuân thủ.

Điểm đáng chú ý nhất của v3 là **deletion vectors**. Chúng thay các positional delete file của v2 bằng các bitmap nén gọn, giúp đọc ghi nhanh hơn với workload update và xóa tần suất cao như xóa bản ghi theo yêu cầu tuân thủ. Vấn đề cũ của v2 là delete file tích tụ dần, mỗi lần đọc engine phải mở và áp dụng tất cả, nên càng ngày càng chậm.

Các tính năng v3 khác: row lineage (theo dõi thay đổi cấp dòng cho CDC), kiểu VARIANT cho dữ liệu bán cấu trúc, giá trị mặc định cho cột, kiểu geometry/geography và timestamp nanosecond.

Về mức độ sẵn sàng: v3 được coi là production-ready kể từ khi Iceberg 1.11.0 ra mắt ngày 19/05/2026. Về phía AWS, deletion vectors và row lineage chạy được trên Spark trên EMR 7.12, AWS Glue ETL, SageMaker Unified Studio và các bảng Iceberg trong Glue Data Catalog (kể cả S3 Tables). Redshift cũng đã hỗ trợ đọc và ghi bảng v3 từ ngày 31/08/2026. Tài liệu AWS tôi tìm được còn ghi Athena chưa hỗ trợ v3, nhưng đó là bản tài liệu từ đầu năm 2026 nên bạn cần kiểm tra lại trước khi quyết định.

Một lưu ý khi nâng cấp: phải xác nhận mọi engine đang ghi vào bảng đều hỗ trợ v3 trước khi nâng version. Một engine cũ ghi vào bảng v3 có thể gây lỗi, nên hãy nâng từ từ và kiểm tra từng engine.

## 7. Dùng Iceberg trên AWS Glue

Glue Data Catalog có thể làm Iceberg catalog, nên Glue job, Athena, EMR và Redshift cùng thấy một bảng. Với Glue job, bạn bật Iceberg bằng job parameter `--datalake-formats iceberg` và cấu hình catalog:

```python
spark = (SparkSession.builder
    .config("spark.sql.extensions",
            "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions")
    .config("spark.sql.catalog.glue_catalog", "org.apache.iceberg.spark.SparkCatalog")
    .config("spark.sql.catalog.glue_catalog.catalog-impl",
            "org.apache.iceberg.aws.glue.GlueCatalog")
    .config("spark.sql.catalog.glue_catalog.warehouse", "s3://bucket/warehouse/")
    .getOrCreate())
```

Tạo bảng, upsert, time travel:

```sql
-- Tạo bảng (partition ẩn theo ngày)
CREATE TABLE glue_catalog.sales_db.orders (
    order_id bigint, country string, amount double, order_ts timestamp)
USING iceberg
PARTITIONED BY (days(order_ts))
TBLPROPERTIES ('format-version' = '2');   -- đặt '3' để dùng v3

-- Upsert từ bảng staging
MERGE INTO glue_catalog.sales_db.orders t
USING updates s ON t.order_id = s.order_id
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *;

-- Time travel
SELECT * FROM glue_catalog.sales_db.orders TIMESTAMP AS OF '2026-11-01 00:00:00';

-- Xem metadata
SELECT * FROM glue_catalog.sales_db.orders.snapshots;
SELECT * FROM glue_catalog.sales_db.orders.files;
```

Để tạo bảng v3, bạn đặt thuộc tính `format-version` bằng 3. Bảng v2 cũng có thể nâng cấp tại chỗ bằng cách đặt lại thuộc tính này.

Bảo trì định kỳ (ví dụ qua Spark procedure):

```sql
CALL glue_catalog.system.rewrite_data_files('sales_db.orders');            -- gộp file nhỏ
CALL glue_catalog.system.expire_snapshots('sales_db.orders', TIMESTAMP '2026-10-01 00:00:00');
CALL glue_catalog.system.remove_orphan_files('sales_db.orders');           -- xóa file mồ côi
```

**Liên hệ với Crawler:** bảng Iceberg **không cần crawler**, vì schema và danh sách file đã nằm trong metadata của chính Iceberg. Đây là lý do mình đã nói ở phần Crawler rằng với Iceberg bạn có thể bỏ crawler.

## 8. Liên hệ với kiến thức Spark Day 1

|Khái niệm Day 1|Gặp lại trong Iceberg|
|---|---|
|Small files / too many partitions|Mỗi lần ghi nhỏ tạo file nhỏ, cần compaction (`rewrite_data_files`)|
|Shuffle là wide transformation|`MERGE INTO` thực chất là join theo key, nên gây shuffle|
|Skew|Key lệch làm `MERGE` chậm, y như `join` thông thường|
|Object storage immutable|Iceberg không sửa file, nó ghi file mới rồi đổi con trỏ metadata|
|Input partition theo kích thước file|File Iceberg có kích thước hợp lý (khoảng 128 đến 512 MB) giúp số task cân đối|

## 9. So sánh nhanh với các table format khác

||Iceberg|Delta Lake|Hudi|Hive-style|
|---|---|---|---|---|
|ACID|Có|Có|Có|Không|
|Time travel|Có|Có|Có|Không|
|Đa engine|Rất tốt (nhiều engine, nhiều vendor)|Tốt, mạnh nhất trên Databricks|Tốt|Tốt|
|Partition evolution|Có, không cần viết lại|Hạn chế hơn|Hạn chế hơn|Không|
|Thế mạnh|Chuẩn mở, trung lập engine|Gắn chặt hệ sinh thái Databricks|Upsert và streaming|Đơn giản|

Đây là so sánh khái quát của mình. Ranh giới giữa các format đang thu hẹp dần, ví dụ Databricks mã hóa deletion vector của Iceberg v3 theo cùng định dạng nhị phân với Delta Lake, cho phép tương tác giữa hai định dạng mà không cần di chuyển dữ liệu.

## 10. Cạm bẫy thường gặp

- **Không bảo trì:** file nhỏ và snapshot tích tụ làm bảng chậm và tốn dung lượng S3. Hãy lập lịch compaction và `expire_snapshots`.
- **Merge-on-read không compaction:** delete file (v2) tích tụ khiến đọc chậm dần.
- **Nhiều writer cùng ghi một bảng:** gây xung đột commit. Cần thiết kế để tránh hoặc xử lý retry.
- **Chọn partition quá mịn:** vẫn tạo ra nhiều file nhỏ như ở Day 1, dù partition là "ẩn".
- **Nâng v3 khi còn engine cũ:** engine không hiểu v3 có thể lỗi khi ghi.
- **Xóa file S3 thủ công:** phá vỡ metadata. Chỉ dọn dẹp bằng các thủ tục của Iceberg.
- **Quên `expire_snapshots`:** time travel giữ lại mọi file cũ, nên không dọn thì dung lượng chỉ tăng.