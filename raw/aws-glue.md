# What is Glue

Glue is a data integration serverless trên AWS

Glue - cluster, master, worker ≠ User - code, transformation logic

# Các service trong Glue

|Thành phần|Vai trò|Hình dung|
|---|---|---|
|**Data Catalog**|Kho metadata trung tâm: database, table, schema, partition, vị trí dữ liệu trên S3|“Mục lục” của data lake, giống Hive Metastore|
|**Crawler**|Quét nguồn dữ liệu, tự suy ra schema và đăng ký table vào Catalog|Robot lập mục lục|
|**ETL Job**|Chạy code transform: Spark, Streaming, Python Shell, Ray|Phần lõi, chính là Spark application|
|**Glue Studio**|Giao diện kéo thả và editor để tạo job|IDE trên web|
|**Interactive Sessions / Notebook**|Chạy và debug code Spark theo từng cell|Jupyter nối với Glue|
|**Workflows và Triggers**|Điều phối chuỗi crawler và job (theo lịch, theo sự kiện, theo điều kiện)|Orchestrator nhẹ|
|**Connections / Connectors**|Lưu thông tin kết nối tới JDBC, Redshift, Kafka, Snowflake...|Kho credential|
|**Data Quality**|Định nghĩa rule kiểm tra chất lượng dữ liệu|Kiểm tra null, trùng lặp, phạm vi giá trị|
|**Schema Registry**|Quản lý và kiểm soát schema cho streaming (Kafka, Kinesis)|Chống đổi schema gây vỡ pipeline|
|**DataBrew**|Làm sạch dữ liệu không cần code|Dành cho analyst|

# Glue Job

|Khái niệm Spark|Trong Glue|
|---|---|
|Application|1 lần chạy job (job run)|
|Master, cluster manager|AWS quản lý, bạn không thấy|
|Driver|Chạy trên 1 worker|
|Executor|Chạy trên các worker còn lại|
|Worker (máy)|Đơn vị tài nguyên gọi là **DPU**|
|Executor memory, cores|Quyết định bởi **worker type**|

# DPU

**DPU** (Data Processing Unit) là đơn vị tài nguyên của Glue: 1 DPU tương đương 4 vCPU và 16 GB RAM. Bạn chọn:

- **Worker type:** G.1X (1 DPU), G.2X (2 DPU), G.4X, G.8X... Worker lớn hơn thì mỗi executor có nhiều core và memory hơn, phù hợp job nhiều shuffle hoặc partition lớn.
- **Số worker:** cố định, hoặc bật **auto scaling** để Glue tự tăng giảm theo khối lượng.

|Worker type|DPU|vCPU|RAM|Hợp với|
|---|---|---|---|---|
|G.1X|1|4|16 GB|Transform, join thông thường|
|G.2X|2|8|32 GB|Transform vừa, ML|
|G.4X|4|16|64 GB|Aggregation lớn|
|G.8X|8|32|128 GB|Workload nặng nhất|
|G.025X|0.25|2|4 GB|Streaming khối lượng thấp|

# DynamicFrame và DataFrame

Glue có cấu trúc dữ liệu riêng, **DynamicFrame**, ngoài DataFrame của Spark:

||DataFrame|DynamicFrame|
|---|---|---|
|Schema|Cố định, mọi dòng cùng schema|Linh hoạt, mỗi record tự mô tả|
|Cột lẫn kiểu (ví dụ `int` và `string`)|Phải xử lý trước|Có kiểu `choice`, xử lý bằng `resolveChoice`|
|Use case|Dữ liệu sạch, nhiều tối ưu của Spark|Dữ liệu bẩn, schema không ổn định|

# Các tính năng quan trọng khi làm thực tế

- **Job bookmarks:** nhớ dữ liệu đã xử lý, lần chạy sau chỉ lấy phần mới (incremental load).
- **Pushdown predicate:** lọc theo partition ngay lúc đọc từ Catalog để không quét cả bảng, một dạng partition pruning.
- **Grouping small files** (`groupFiles`): gộp nhiều file nhỏ thành ít partition, xử lý vấn đề “small file problem” từ bài về partitioning.
- **Job parameters:** truyền tham số lúc chạy (ngày xử lý, đường dẫn...).
- **Open table formats:** Iceberg, Hudi, Delta cho upsert, delete và time travel trên S3.
- **Flex execution:** chạy trên tài nguyên dư của AWS với giá rẻ hơn, hợp job không gấp.
- **Spark UI và CloudWatch:** xem stage, shuffle, spill. FOR CHECKLIST DEBUG

# Cost

Glue tính tiền theo **DPU-giờ**, áp dụng cho job và crawler, tính theo thời gian chạy.

Data Catalog tính riêng theo số object lưu trữ và số request.

Một cách nghĩ hữu ích: **tiền = số DPU × thời gian chạy**, nên job chậm do shuffle hay skew đều tốn tiền trực tiếp.

reference: [https://aws.amazon.com/vi/glue/pricing/](https://aws.amazon.com/vi/glue/pricing/)

# Data Catalog & Crawler

## 1. Data Catalog: kho metadata, không chứa dữ liệu

Data Catalog chỉ lưu **thông tin mô tả** về dữ liệu: dữ liệu nằm ở đâu, định dạng gì, schema ra sao. Bản thân dữ liệu vẫn nằm trên S3 hoặc nguồn gốc. Nó tương đương Hive Metastore, và được mô tả là kho metadata tập trung, được quản lý hoàn toàn, đóng vai trò "bộ não" của data lake.

### Cấu trúc phân cấp

```
Data Catalog (1 cái cho mỗi account + region)
 └── Database          (nhóm logic các table, không phải DB thật)
      └── Table        (schema + vị trí dữ liệu + format)
           └── Partition   (từng "thư mục con" như year=2026/month=11)
```

Một table trong Catalog lưu:

|Thông tin|Ví dụ|
|---|---|
|Location|`s3://bucket/orders/`|
|Schema|`order_id bigint, country string, amount double`|
|Format và SerDe|Parquet, CSV, JSON...|
|Partition keys|`year`, `month`|
|Properties|`classification=parquet`, kiểu nén...|

Table trong Catalog là dạng **external**: xóa table chỉ xóa metadata, không xóa file trên S3.

### Tại sao nó quan trọng

Nhiều engine dùng chung một Catalog: Glue job, Athena, Redshift Spectrum, EMR, Lake Formation. Schema khai báo một lần là mọi nơi cùng thấy. Catalog còn hỗ trợ versioning schema và rollback.

### Partition và liên hệ với Spark

Dữ liệu trên S3 thường tổ chức như sau:

```
s3://bucket/orders/year=2026/month=10/part-000.parquet
s3://bucket/orders/year=2026/month=11/part-000.parquet
```

Mỗi thư mục là một **partition entry** trong Catalog. Khi query có `WHERE year=2026 AND month=11`, engine chỉ đọc đúng partition đó (**partition pruning**) thay vì quét cả bảng. Trong Glue, bạn dùng `push_down_predicate` để lọc partition ngay lúc đọc.

Đừng nhầm hai loại partition:

||Partition trong Catalog|Spark partition|
|---|---|---|
|Là gì|Cách tổ chức thư mục trên S3|Đơn vị công việc trong bộ nhớ|
|Chia theo|Giá trị cột (năm, tháng...)|Kích thước dữ liệu hoặc hash key|
|Mục đích|Bỏ qua dữ liệu không cần đọc|Chạy song song|

Hai loại này liên quan nhau: số file trong các partition trên S3 ảnh hưởng số **input partition** của Spark. Nhiều file nhỏ thì Spark tạo nhiều partition nhỏ, đúng như bài về "too many partitions".

### Dùng Catalog trong Glue job

```python
dyf = glueContext.create_dynamic_frame.from_catalog(
    database="sales_db",
    table_name="orders",
    push_down_predicate="year == '2026' and month == '11'"
)
df = dyf.toDF()   # chuyển sang Spark DataFrame
```

Hoặc dùng Catalog như Hive Metastore: `spark.sql("SELECT * FROM sales_db.orders")`.

## 2. Crawler: robot lập mục lục infer_schema

Crawler quét các nguồn như S3, JDBC, DynamoDB, tự suy ra tên cột, kiểu dữ liệu, partition và định dạng file, rồi tạo hoặc cập nhật table trong Data Catalog.

### Crawler chạy như thế nào

```
1. Kết nối nguồn dữ liệu (bằng IAM role)
2. Classifier thử nhận diện định dạng (CSV, JSON, Parquet, Avro, ORC...)
3. Suy ra schema từ file
4. Gom file thành table (theo cấu trúc thư mục và độ giống schema)
5. Ghi hoặc cập nhật table và partition vào Catalog
```

Mỗi đường dẫn S3 thường trở thành một table riêng, còn với JDBC thì một crawler có thể phát hiện nhiều table từ một database.

Các thành phần cấu hình chính:

|Thành phần|Ý nghĩa|
|---|---|
|Data source|S3 path, JDBC, DynamoDB...|
|IAM role|Quyền đọc nguồn và ghi Catalog|
|Classifier|Luật nhận diện định dạng (có sẵn hoặc tự viết bằng grok/regex/JSON/CSV)|
|Target database|Database trong Catalog nhận kết quả|
|Table prefix|Tiền tố tên table, ví dụ `raw_`|
|Exclusions|Pattern bỏ qua (file tạm, `_SUCCESS`...)|
|Schedule|Chạy theo lịch hoặc on-demand|
|Schema change policy|Xử lý khi schema đổi|
|Recrawl policy|Quét lại toàn bộ hay chỉ phần mới|

### Schema change policy

Quy định hành vi khi crawler thấy schema khác với Catalog. `UpdateBehavior` có thể là `UPDATE_IN_DATABASE` (ghi đè schema trong Catalog) hoặc `LOG` (chỉ ghi log). `DeleteBehavior` điều khiển việc xử lý object đã biến mất khỏi nguồn (ghi log, xóa khỏi Catalog, hoặc đánh dấu deprecated).

Với pipeline production, `LOG` thường an toàn hơn: bạn không muốn một file lỗi làm đổi schema table mà các job phía sau đang phụ thuộc.

### Recrawl policy

|Chế độ|Hành vi|Khi nào dùng|
|---|---|---|
|`CRAWL_EVERYTHING`|Quét lại toàn bộ mỗi lần|Dữ liệu nhỏ, hoặc file cũ có thể thay đổi|
|`CRAWL_NEW_FOLDERS_ONLY`|Chỉ quét thư mục mới|Dữ liệu append-only, rất nhanh hơn|
|`CRAWL_EVENT_MODE`|Dựa trên sự kiện S3 để biết chỗ nào đổi|Dữ liệu lớn, cần cập nhật gần thời gian thực|

Về incremental crawl, cần lưu ý ba điều từ tài liệu AWS:

- Lần chạy đầu tiên luôn là full crawl, ghi lại toàn bộ schema và mọi partition hiện có.
- Incremental crawl không phát hiện việc sửa hay xóa partition đã có, nên chỉ hợp với nguồn có schema ổn định.
- Nếu gặp object có schema không tương thích, crawler không thêm vào Catalog mà ghi log vào CloudWatch Logs. Vì vậy, khi thấy table thiếu dữ liệu, hãy kiểm tra log này.

### Tạo crawler bằng code

```python
import boto3
glue = boto3.client("glue", region_name="ap-southeast-1")

glue.create_crawler(
    Name="orders-crawler",
    Role="arn:aws:iam::123456789012:role/GlueCrawlerRole",
    DatabaseName="sales_db",
    Targets={"S3Targets": [{"Path": "s3://bucket/orders/"}]},
    SchemaChangePolicy={"UpdateBehavior": "LOG", "DeleteBehavior": "LOG"},
    RecrawlPolicy={"RecrawlBehavior": "CRAWL_NEW_FOLDERS_ONLY"},
)
glue.start_crawler(Name="orders-crawler")
```

## 3. Cạm bẫy thường gặp

- **Suy luận schema sai khi dữ liệu bẩn.** Cột có lúc là số, lúc là chuỗi thì crawler có thể chọn `string` hoặc tạo nhiều table. Đây là lý do Glue có DynamicFrame với kiểu `choice`.
- **Một thư mục thành nhiều table.** Nếu file trong cùng thư mục có schema quá khác nhau, crawler tách thành nhiều table. Hãy giữ mỗi thư mục một schema.
- **Quên partition mới.** Dữ liệu mới vào thư mục mới nhưng chưa có partition entry thì query sẽ không thấy. Cần chạy crawler hoặc thêm partition.
- **Tốn tiền và thời gian.** Crawler tính tiền theo DPU-giờ như job, nên crawl toàn bộ bucket lớn mỗi giờ là lãng phí.

## 4. Khi nào nên bỏ crawler

Crawler rất tiện để khám phá dữ liệu lạ, nhưng với pipeline production có schema đã biết, nhiều đội **tự định nghĩa table** (bằng Athena DDL, Terraform, hoặc `create_table` API) và thêm partition chủ động. Lý do là schema trở thành "hợp đồng" rõ ràng, tránh việc crawler tự đổi schema và làm hỏng job phía sau.

Với bảng **Iceberg, Hudi, Delta**, bạn không cần crawler để theo dõi schema, vì chính table format đã quản lý metadata và schema evolution.

## 5. Bài tập thực hành

1. Tạo bucket S3 với dữ liệu Parquet chia partition `year=/month=`.
2. Tạo crawler trỏ vào đó, chạy và xem table sinh ra trong Catalog (schema, location, partition).
3. Query bằng Athena để thấy cùng Catalog được dùng chung.
4. Thêm thư mục tháng mới, chạy lại crawler ở hai chế độ `CRAWL_EVERYTHING` và `CRAWL_NEW_FOLDERS_ONLY`, so sánh thời gian.
5. Cố tình thêm file có schema khác (đổi kiểu một cột) và quan sát phản ứng của crawler với từng schema change policy.

# Glue Job

Glue Job là phần lõi của Glue, và với Spark job thì nó chính là **một Spark application** như ở Day 1. Khác biệt là bạn không tự dựng cluster, master hay executor: bạn chỉ khai báo script và tài nguyên, Glue lo phần còn lại.

## 1. Job definition và Job run

||Job definition|Job run|
|---|---|---|
|Là gì|Cấu hình đã lưu: script, role, version, worker type, số worker, timeout, tham số|Một lần chạy cụ thể của job|
|Tương đương|"Bản thiết kế"|1 lần `spark-submit` (1 Spark application)|
|Vòng đời|Tồn tại cho đến khi bạn xóa|Khởi động, chạy, kết thúc, các executor biến mất|

Mỗi lần job chạy, Glue cấp tài nguyên, khởi chạy driver và executor, chạy script rồi giải phóng. Điều này khớp với bài học: executor là tiến trình tạm thời, chỉ tồn tại trong một application.

Các loại job:

- **Spark (`glueetl`):** loại chính, batch bằng PySpark hoặc Scala.
- **Spark Streaming (`gluestreaming`):** đọc Kafka hoặc Kinesis, xử lý theo micro-batch.
- **Python Shell:** script Python thuần cho việc nhẹ.
- **Ray:** workload Python phân tán, ít gặp hơn.

## 2. Worker type và DPU

DPU là thước đo tương đối sức xử lý, gồm 4 vCPU và 16 GB bộ nhớ. Bạn chọn **worker type** và **số worker**:

|Worker type|DPU|vCPU|RAM|Hợp với|
|---|---|---|---|---|
|G.1X|1|4|16 GB|Transform, join thông thường|
|G.2X|2|8|32 GB|Transform vừa, ML|
|G.4X|4|16|64 GB|Aggregation lớn|
|G.8X|8|32|128 GB|Workload nặng nhất|
|G.025X|0.25|2|4 GB|Streaming khối lượng thấp|

Thông số G.1X đến G.8X lấy từ tài liệu AWS, và mỗi worker cung cấp 1 executor. G.025X chỉ dùng cho streaming job từ Glue 3.0 trở lên. Ngoài ra, có các worker loại R (R.1X đến R.8X) với tỉ lệ vCPU:memory là 1:8 thay vì 1:4 của loại G, nguồn này là một bài hướng dẫn bên thứ ba nên bạn nên kiểm tra lại với tài liệu AWS trước khi dùng.

### Ví dụ tính song song (nối với Day 1)

```
Job: 10 worker G.1X
→ thường 1 worker làm driver, 9 worker còn lại là executor (mỗi cái 4 core)
→ 9 executor × 4 core = 36 task chạy song song
```

Con số "1 worker cho driver" là cách hoạt động thường thấy mà bạn nên tự kiểm chứng trong Spark UI tab Executors. Nếu stage có 100 task thì chạy khoảng 3 đợt (36, 36, 28).

Vì thế chọn worker type chính là chọn **memory mỗi executor và số task song song mỗi executor**:

- Partition lớn, hay spill ra đĩa, OOM: tăng worker type (G.2X, G.4X) hoặc dùng loại R.
- Job nhiều task nhỏ: tăng số worker.
- Skew: thêm worker không giải quyết được, vì một task vẫn ôm hết dữ liệu của key lệch.

## 3. Khung script Glue PySpark

```python
import sys
from awsglue.utils import getResolvedOptions
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglue.dynamicframe import DynamicFrame
from pyspark.context import SparkContext

args = getResolvedOptions(sys.argv, ["JOB_NAME", "process_date"])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args["JOB_NAME"], args)

# 1. Đọc từ Data Catalog, chỉ lấy partition cần thiết
dyf = glueContext.create_dynamic_frame.from_catalog(
    database="sales_db",
    table_name="orders",
    push_down_predicate=f"dt == '{args['process_date']}'",
    transformation_ctx="read_orders",       # cần cho job bookmark
)

# 2. Chuyển sang DataFrame để dùng Spark thuần
df = dyf.toDF().filter("status = 'completed'").select("country", "amount")
result = df.groupBy("country").sum("amount")

# 3. Ghi ra S3
out = DynamicFrame.fromDF(result, glueContext, "out")
glueContext.write_dynamic_frame.from_options(
    frame=out,
    connection_type="s3",
    connection_options={"path": "s3://bucket/revenue_by_country/"},
    format="parquet",
    transformation_ctx="write_out",
)

job.commit()   # lưu trạng thái bookmark
```

Đây gần như là job ở ví dụ Day 1: `write_dynamic_frame` là **action** tạo job, `groupBy` là **wide transformation** cắt stage, còn `filter` và `select` là narrow nên chạy chung stage 0.

Ba dòng mở đầu và `job.commit()` là khung bắt buộc của Glue:

- `getResolvedOptions`: lấy tham số (`-process_date`) truyền khi chạy job.
- `job.init` và `job.commit`: đăng ký và chốt trạng thái job, cần cho bookmark và theo dõi.

## 4. DynamicFrame, DataFrame và cách dùng cùng nhau

||DataFrame|DynamicFrame|
|---|---|---|
|Schema|Cố định|Linh hoạt, mỗi record tự mô tả|
|Cột lẫn kiểu|Phải xử lý trước|Kiểu `choice`, xử lý bằng `resolveChoice`|
|Tối ưu Catalyst|Đầy đủ|Hạn chế|
|Dùng khi|Dữ liệu sạch, transform phức tạp|Đọc dữ liệu bẩn, ghi qua Glue|

Mẫu thường dùng: **đọc bằng DynamicFrame** (chịu được dữ liệu bẩn) → `resolveChoice` để chốt kiểu → `toDF()` để làm việc bằng Spark SQL/DataFrame → `fromDF()` nếu cần ghi bằng API của Glue.

## 5. Các tính năng quan trọng của job

**Job bookmark.** Glue nhớ dữ liệu đã xử lý, lần chạy sau chỉ lấy phần mới (incremental). Muốn bookmark hoạt động bạn cần bật nó trong job, đặt `transformation_ctx` cho từng bước đọc và ghi, và gọi `job.commit()`. Thiếu một trong ba thì job có thể đọc lại toàn bộ dữ liệu.

**Pushdown predicate.** Lọc theo partition ngay lúc đọc để không quét cả bảng. Khác với `filter` trong Spark (lọc sau khi đã đọc), predicate này loại bỏ partition từ bước liệt kê nên tiết kiệm cả I/O lẫn thời gian.

**Gộp file nhỏ (`groupFiles`).** Khi nguồn có hàng chục nghìn file nhỏ, Glue gộp chúng vào ít partition hơn. Đây là cách xử lý vấn đề "too many partitions" ở bài Partitioning.

**Job parameters.** Truyền ngày xử lý, đường dẫn, cờ cấu hình lúc chạy. Lưu ý tham số job có thể bị ghi vào log, nên đừng truyền secret dạng văn bản thô, hãy lấy từ Secrets Manager hoặc Glue Connection.

**Auto scaling.** Từ Glue 3.0, Glue có thể tự điều chỉnh số worker. Bạn đặt số worker tối đa, Glue tăng giảm theo khối lượng thực. Nên theo dõi số executor đang hoạt động trong CloudWatch để biết đặt max có quá tay không. Ví dụ nếu đỉnh chỉ dùng 15 trên 50 worker thì nên hạ max xuống.

**Flex execution.** Chế độ Flex chỉ cho Spark job (`glueetl`) từ Glue 3.0 trở lên, còn chế độ Standard dành cho workload cần khởi động nhanh và tài nguyên riêng. Flex rẻ hơn nhưng có thể bắt đầu trễ hơn, hợp với batch ban đêm hoặc backfill.

**Timeout.** Mặc định là 48 giờ, một job lỗi vòng lặp có thể chạy rất lâu và tốn tiền. Hãy đặt timeout rõ ràng cho từng job.

## 6. Theo dõi và debug

- **Spark UI:** bật qua job parameter, xem stage, task, shuffle read/write, spill, độ lệch thời gian giữa các task. Đây là nơi áp dụng checklist debug Day 1.
- **CloudWatch:** log driver và executor, các metric như số executor đang hoạt động.
- **Job run insights:** gợi ý lỗi và nguyên nhân khi job fail.

Quy trình debug khi job Glue chậm vẫn như Day 1: tìm stage đắt nhất → kiểm tra skew → xem số và kích thước partition → kiểm tra join → kiểm tra spill. Khác biệt duy nhất là "tăng tài nguyên" nghĩa là đổi worker type hoặc số worker trong cấu hình job.

## 7. Cạm bẫy thường gặp

- **Tăng worker để chữa skew:** tốn tiền mà không nhanh hơn, vì một task vẫn gánh phần lệch.
- **Quên `transformation_ctx`:** bookmark không hoạt động, job đọc lại toàn bộ.
- **Không đặt timeout:** job treo có thể chạy hàng giờ liền.
- **Ghi quá nhiều file nhỏ:** do shuffle partition quá nhiều. Cân nhắc `coalesce` trước khi ghi hoặc bật AQE.
- **Lạm dụng DynamicFrame cho logic phức tạp:** chuyển sang DataFrame để được Catalyst tối ưu.
- **Giữ job trên Glue version cũ:** các bản cũ đã hoặc sắp hết vòng đời. Glue 6.0 còn giảm giá, nên việc nâng version thường vừa nhanh hơn vừa rẻ hơn.

## 8. Bài tập thực hành

1. Tạo job đọc table `orders` từ Catalog (bài trước), lọc, `groupBy`, ghi Parquet ra S3.
2. Chạy với 2 cấu hình: 5 × G.1X và 10 × G.1X. So sánh thời gian và số executor trong Spark UI.
3. Bật job bookmark, thêm một partition mới, chạy lại và xác nhận chỉ dữ liệu mới được đọc.
4. Thử bật auto scaling và quan sát số executor tăng giảm.
5. Cố tình tạo skew (một country chiếm đa số) và tìm task chậm trong Spark UI.

# Glue Workflows và Triggers

Đến giờ bạn đã có crawler và job riêng lẻ. Pipeline thực tế thường là một chuỗi, ví dụ: quét dữ liệu thô → làm sạch → tổng hợp → cập nhật catalog. Workflows và Triggers giúp chạy chuỗi này tự động, đúng thứ tự.

## 1. Workflow là gì

Glue Workflow là dịch vụ điều phối để tạo, quản lý và theo dõi các hoạt động ETL phức tạp gồm nhiều crawler, job và trigger. Nó cho phép quản lý các job và crawler phụ thuộc nhau như một đơn vị duy nhất, giúp dễ điều phối và theo dõi pipeline.

Trong console, workflow hiển thị dưới dạng đồ thị các bước và trạng thái từng lần chạy, nên bạn thấy ngay bước nào đã xong, bước nào lỗi.

## 2. Trigger: thứ khởi động mọi thứ

Trigger có 4 loại: SCHEDULED, CONDITIONAL, ON_DEMAND và EVENT.

|Loại|Khi nào kích hoạt|Ví dụ|
|---|---|---|
|**On-demand**|Bạn bấm chạy hoặc gọi API|Chạy thử, backfill|
|**Scheduled**|Theo lịch cron|Mỗi ngày 01:00|
|**Conditional**|Khi các job hoặc crawler trước đó đạt trạng thái nhất định|Job B chạy sau khi job A `SUCCEEDED`|
|**Event**|Khi nhận sự kiện từ EventBridge|File mới đổ vào S3|

Một vài chi tiết đáng nhớ:

- Trigger scheduled bắt buộc có trường `Schedule`, còn trigger conditional bắt buộc có `Predicate` (điều kiện kích hoạt).
- `StartOnCreation = true` dùng được cho trigger scheduled và conditional, nhưng không hỗ trợ cho on-demand. Quên bật là lý do phổ biến khiến pipeline "không tự chạy".
- Với Event trigger, `BatchSize` nằm trong khoảng 1 đến 100: số sự kiện EventBridge phải nhận được trước khi trigger bắn. Ngoài số lượng, điều kiện gom batch còn có thể là hết một khoảng thời gian chờ. Cơ chế này tránh việc 1.000 file đến liên tiếp tạo ra 1.000 lần chạy job.

### Predicate của conditional trigger

Predicate là tập điều kiện kết hợp bằng `AND` hoặc `ANY`, mỗi điều kiện kiểm tra trạng thái của một job (`SUCCEEDED`, `FAILED`...) hoặc một crawler. Nhờ đó bạn có thể rẽ nhánh:

```
Job A ─ SUCCEEDED ─► Job B (xử lý tiếp)
      └ FAILED    ─► Job Alert (gửi thông báo)
```

## 3. Ví dụ pipeline

```
[Scheduled 01:00]
      │
      ▼
 Crawler: raw-crawler ──(SUCCEEDED)──► Job: clean-orders ──(SUCCEEDED)──► Job: aggregate-revenue
                                                                              │
                                                             (SUCCEEDED)      ▼
                                                                     Crawler: curated-crawler
```

Mỗi mũi tên là một conditional trigger, điểm bắt đầu là một scheduled trigger.

```python
import boto3
glue = boto3.client("glue")

glue.create_workflow(Name="orders-pipeline", MaxConcurrentRuns=1)

# 1) Bắt đầu theo lịch, chạy crawler
glue.create_trigger(
    Name="start-0100", WorkflowName="orders-pipeline",
    Type="SCHEDULED", Schedule="cron(0 1 * * ? *)",
    Actions=[{"CrawlerName": "raw-crawler"}],
    StartOnCreation=True,
)

# 2) Crawler xong thì chạy job làm sạch
glue.create_trigger(
    Name="after-crawl", WorkflowName="orders-pipeline",
    Type="CONDITIONAL",
    Predicate={"Logical": "AND", "Conditions": [
        {"LogicalOperator": "EQUALS", "CrawlerName": "raw-crawler", "CrawlState": "SUCCEEDED"}
    ]},
    Actions=[{"JobName": "clean-orders"}],
    StartOnCreation=True,
)

# 3) Job làm sạch xong thì chạy job tổng hợp
glue.create_trigger(
    Name="after-clean", WorkflowName="orders-pipeline",
    Type="CONDITIONAL",
    Predicate={"Logical": "AND", "Conditions": [
        {"LogicalOperator": "EQUALS", "JobName": "clean-orders", "State": "SUCCEEDED"}
    ]},
    Actions=[{"JobName": "aggregate-revenue"}],
    StartOnCreation=True,
)
```

`MaxConcurrentRuns=1` ngăn hai lần chạy của cùng workflow chồng lên nhau (ví dụ lần hôm qua chưa xong mà lần hôm nay đã bắt đầu). Workflow có thuộc tính số lần chạy đồng thời tối đa, và cả `default_run_properties`.

## 4. Run properties: truyền dữ liệu giữa các bước

Các job trong workflow chạy độc lập, mỗi job là một Spark application riêng nên không chia sẻ biến với nhau. **Workflow run properties** là các cặp key-value gắn với một lần chạy workflow. Bước đầu có thể ghi vào (ví dụ `process_date`, đường dẫn output), bước sau đọc ra.

Trong script, job lấy tên và ID của lần chạy qua tham số (`WORKFLOW_NAME`, `WORKFLOW_RUN_ID`), rồi gọi API `get_workflow_run_properties` để đọc. Nhờ vậy cả pipeline dùng chung một ngày xử lý mà không phải hard-code từng job.

## 5. Điểm cần lưu ý về hiệu năng

Mỗi job trong workflow là một Spark application riêng, nên mỗi job đều phải trả chi phí khởi động (xin tài nguyên, dựng driver và executor). Pipeline 5 job nhỏ sẽ mất thời gian khởi động 5 lần, và dữ liệu trung gian phải ghi ra S3 rồi đọc lại giữa các job.

Nguyên tắc: các bước **chỉ gồm narrow transformation nối tiếp** thường nên gộp vào cùng một job. Chỉ tách job khi có lý do rõ ràng: cần kiểm tra chất lượng giữa chừng, cần chạy lại riêng từng bước, hoặc hai bước cần cấu hình tài nguyên khác hẳn nhau.

## 6. Khi nào dùng Glue Workflow, khi nào dùng công cụ khác

||Glue Workflow|Step Functions|Airflow (MWAA)|
|---|---|---|---|
|Phạm vi|Chỉ crawler và job của Glue|Nhiều service AWS|Mọi hệ thống|
|Độ phức tạp logic|Đơn giản (nối tiếp, rẽ nhánh theo trạng thái)|Rẽ nhánh, lặp, xử lý lỗi, chạy song song|Rất linh hoạt, DAG lớn|
|Chi phí vận hành|Thấp, không cần dựng gì thêm|Thấp|Cao hơn (có môi trường để quản lý)|
|Hợp với|Pipeline thuần Glue, đội nhỏ|Pipeline trộn Glue, Lambda, SNS...|Đội lớn, nhiều pipeline, cần tích hợp ngoài AWS|

Đây là phần đánh giá chung của mình, không phải từ tài liệu AWS, nên hãy cân nhắc theo bối cảnh của bạn. Nếu pipeline chỉ gồm vài crawler và job của Glue thì Glue Workflow là đủ và rẻ nhất. Khi logic bắt đầu có nhiều rẽ nhánh hoặc phải gọi nhiều service ngoài Glue, hãy cân nhắc Step Functions hoặc Airflow.

## 7. Cạm bẫy thường gặp

- **Quên bật trigger:** trigger tạo ra nhưng chưa được kích hoạt nên workflow không bao giờ bắt đầu.
- **Không có nhánh xử lý khi lỗi:** job fail, các bước sau không chạy và không ai biết. Nên thêm nhánh `FAILED` gửi thông báo.
- **Chạy chồng lên nhau:** không giới hạn số lần chạy đồng thời, hai lần chạy ghi đè lên cùng đường dẫn output.
- **Hard-code ngày xử lý trong từng job:** các job lệch ngày nhau khi chạy qua nửa đêm. Hãy truyền ngày qua run properties.
- **Event trigger không gom batch:** mỗi file nhỏ kích hoạt một lần chạy, tốn tiền khởi động.
- **Tách job quá mức:** nhiều job nhỏ làm tăng thời gian khởi động và I/O trung gian.

## 8. Bài tập thực hành

1. Tạo workflow gồm: scheduled trigger → crawler → job (từ bài trước) → crawler.
2. Chạy thủ công bằng on-demand rồi xem đồ thị trạng thái trong console.
3. Cố tình làm job fail và xem phần sau của pipeline phản ứng ra sao. Sau đó thêm một nhánh `FAILED` chạy job gửi thông báo.
4. Đặt `MaxConcurrentRuns=1`, bấm chạy hai lần liên tiếp và quan sát lần thứ hai bị chặn.
5. Thử truyền `process_date` qua run properties cho cả hai job.