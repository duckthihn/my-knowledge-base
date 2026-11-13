
is a serveless, interactive query service from AWS that lets you analyze data directly in Amazon S3 using standard SQL

**Glue Integration:** Connects seamlessly with the [**AWS Glue Data Catalog**](https://aws.amazon.com/glue/) to manage schemas and metadata.

**Pay-per-Query:** You pay only for the amount of data scanned by your queries (typically around $5 per TB scanned).

Use case: Query, light BI, ad-hoc

**Key takeaways**: dữ liệu Parquet trên S3 và một table trong Glue Data Catalog, viết SQL, và có kết quả mà không dựng gì cả. Đây là điểm khác lớn nhất với Spark trên Glue, phải viết job, chọn worker và chờ job khởi động.

## How it works

```python
	SQL
   │
   ▼
Athena (Trino: coordinator + worker do AWS quản lý, bạn không thấy)
   │  1. Lấy schema, vị trí, partition từ Glue Data Catalog
   │  2. Lập kế hoạch, bỏ qua partition và cột không cần
   │  3. Worker đọc file từ S3 song song
   ▼
Kết quả ghi ra S3 (query result location) và trả về cho bạn
```

Về mặt kiến trúc, Trino là MPP (nhiều worker xử lý song song, trao đổi dữ liệu giữa các stage), gần với Spark ở tư duy chia việc theo split và stage. Điểm khác là bạn **không điều chỉnh được cluster**, nên đòn bẩy duy nhất của bạn là **cách bố trí dữ liệu và cách viết SQL**.

---

## Cost

Athena tính 5 USD mỗi TB quét (giá niêm yết ở us-east-1), làm tròn lên megabyte gần nhất, tối thiểu 10 MB mỗi truy vấn. Truy vấn lỗi, bị hủy và lệnh DDL đều miễn phí, và không tốn tiền khi rảnh.

Athena tính theo byte quét chứ không theo byte trả về: `SELECT COUNT(*)` trên bảng 5 TB vẫn quét 5 TB nếu planner không loại được partition hay cột. Ví dụ chính thức của AWS cho thấy ảnh hưởng của định dạng: với bảng 3 TB dạng text không nén, truy vấn một cột phải quét toàn bộ file vì text không chia được, tốn 15 USD. Cùng bảng ở dạng columnar, quét một cột chỉ 0,25 TB, tốn 1,25 USD.

reference: [https://aws.amazon.com/vi/athena/pricing/](https://aws.amazon.com/vi/athena/pricing/)

**key takeaways:** **chi phí = lượng dữ liệu quét**, nên layout dữ liệu chính là chi phí (partition, file format, …)

## Tối ưu chi phi và tốc độ

|Đòn bẩy|Tác dụng|
|---|---|
|**Định dạng columnar** (Parquet, ORC)|Chỉ đọc các cột được chọn, giảm mạnh lượng quét|
|**Nén** (Snappy, Zstd, Gzip)|Giảm byte đọc từ S3|
|**Partition** theo cột hay lọc (ngày, vùng)|Bỏ qua cả thư mục không liên quan (partition pruning)|
|**Chỉ `SELECT` cột cần dùng**|Tránh `SELECT *` làm đọc mọi cột|
|**Kích thước file hợp lý** (khoảng 128 MB trở lên)|Tránh overhead mở hàng nghìn file nhỏ|
|**Lọc theo cột partition trong `WHERE`**|Cần để pruning hoạt động|
|**Result reuse**|Tái dùng kết quả của truy vấn giống hệt gần đây|

## Partition Projection (dự tính partition)

Với bảng Hive-style có nhiều partition, mỗi truy vấn phải hỏi Glue Catalog danh sách partition, rất chậm khi có hàng chục nghìn partition. Partition projection tính vị trí partition từ một công thức thay vì liệt kê trong catalog, tránh được các lần tra cứu metadata chậm.

Partition projection đảo ngược cách làm: bạn **mô tả quy luật** của các partition trong thuộc tính của bảng, và Athena **tự suy ra các đường dẫn S3** từ quy luật đó cùng với điều kiện `WHERE`, không cần hỏi catalog.

Example:

```python
Cách thường:  WHERE dt = '2026-11-05'
              → hỏi Glue Catalog "partition nào khớp?" → đọc S3

Projection:   WHERE dt = '2026-11-05'
              → tính ra s3://bucket/events/dt=2026-11-05/ → đọc S3
```

### Các kiểu cột partition

|Kiểu|Dùng cho|Cấu hình chính|
|---|---|---|
|`date`|Ngày, giờ, tháng|`range`, `format`, `interval`, `interval.unit`|
|`integer`|Số (năm, ID khối)|`range`, `interval`, `digits`|
|`enum`|Tập giá trị nhỏ, cố định (quốc gia, loại sự kiện)|`values`|
|`injected`|Giá trị quá nhiều để liệt kê (user ID)|Không có range, bắt buộc có điều kiện `=` trong `WHERE`|

### Ví dụ đầy đủ

Athena query editor  
Kiểm tra lại bằng `SHOW TBLPROPERTIES events;`. Lệnh DDL không tính tiền, nên thử nghiệm thoải mái.

```sql
CREATE EXTERNAL TABLE events (
  event_id string,
  payload  string
)
PARTITIONED BY (dt string, country string)
STORED AS PARQUET
LOCATION 's3://bucket/events/'
TBLPROPERTIES (
  'projection.enabled'          = 'true',

  'projection.dt.type'          = 'date',
  'projection.dt.format'        = 'yyyy-MM-dd',
  'projection.dt.range'         = '2025-01-01,NOW',
  'projection.dt.interval'      = '1',
  'projection.dt.interval.unit' = 'DAYS',

  'projection.country.type'     = 'enum',
  'projection.country.values'   = 'VN,US,JP',

  'storage.location.template'   = 's3://bucket/events/dt=${dt}/country=${country}/'
);
```

`NOW` trong `range` nghĩa là khoảng ngày tự mở rộng theo thời gian, nên partition của ngày mai không cần đăng ký.

### Những điểm cần cẩn thận

- **Chỉ Athena hiểu cơ chế này.** Cùng bảng đó, Redshift Spectrum hay EMR (qua Glue Catalog) sẽ không thấy partition nào nếu bạn không đăng ký partition theo cách thường. Nếu bảng dùng chung nhiều dịch vụ thì phải bảo trì partition song song.
- **Luôn lọc theo cột partition.** Athena sinh đường dẫn từ điều kiện lọc; nếu thiếu điều kiện trên một dải rất rộng, nó có thể phải thử một lượng lớn đường dẫn và tốn nhiều request S3.
- **Đường dẫn không tồn tại không gây lỗi.** Partition được suy ra nhưng không có dữ liệu thì đơn giản là trả về rỗng, nên sai `format` hay sai `storage.location.template` thường cho kết quả rỗng thay vì báo lỗi, rất khó phát hiện.
- **Cấu trúc thư mục phải đều.** Quy luật chỉ đúng khi mọi partition đặt tên theo cùng một mẫu.
- **Kiểu `injected` bắt buộc có điều kiện bằng** trên cột đó trong `WHERE`, nếu không truy vấn sẽ lỗi.
- **Không dùng với bảng Iceberg**, vì Iceberg tự quản lý metadata và partition