# Reference

[https://vutr.substack.com/p/heres-everything-you-need-to-know](https://vutr.substack.com/p/heres-everything-you-need-to-know)

# What is Parquet?

**Apache Parquet** là một định dạng lưu trữ dữ liệu dạng **cột (columnar)**, mã nguồn mở, được thiết kế cho xử lý dữ liệu lớn và phân tích (analytics). Nó ra đời khoảng năm 2013 từ sự hợp tác giữa Twitter và Cloudera, lấy ý tưởng từ bài báo Dremel của Google, và hiện là chuẩn gần như mặc định trong hệ sinh thái Hadoop, Spark, data lake và data warehouse.

## Ý tưởng cốt lõi: lưu theo cột thay vì theo hàng

Giả sử bảng có 3 cột `id, tên, tuổi`:

- **Row-based (CSV, JSON, Avro):** `1,An,25 | 2,Bình,30 | 3,Chi,28`
- **Columnar (Parquet):** `1,2,3 | An,Bình,Chi | 25,30,28`

Điều này có lợi vì:

1. **Chỉ đọc cột cần thiết.** Truy vấn `SELECT AVG(tuổi)` chỉ đọc cột `tuổi`, bỏ qua phần còn lại (_column pruning_). Với bảng hàng trăm cột thì tiết kiệm I/O rất lớn.
2. **Nén tốt hơn.** Dữ liệu trong cùng một cột thường cùng kiểu và giống nhau, nên dễ nén hơn nhiều.
3. **Đọc nhanh hơn** nhờ xử lý vector hóa (vectorized) trên từng cột.

## Cấu trúc một file Parquet

```
File
├── Row Group 1        (vài chục đến vài trăm MB)
│   ├── Column Chunk (cột A)
│   │   └── Pages (data page, dictionary page...)
│   └── Column Chunk (cột B) ...
├── Row Group 2 ...
└── Footer             (schema, metadata, thống kê min/max từng chunk)
```

- **Row group:** nhóm các hàng, là đơn vị để xử lý song song.
- **Column chunk:** dữ liệu của một cột trong một row group.
- **Page:** đơn vị nhỏ nhất được mã hóa và nén.
- **Footer:** chứa schema và thống kê, nằm ở cuối file. Reader đọc footer trước để biết cần đọc phần nào.

## Các tính năng quan trọng

- **Encoding thông minh:** dictionary encoding, run-length encoding (RLE), bit-packing, delta encoding.
- **Nén theo từng cột:** Snappy (mặc định, nhanh), Gzip, ZSTD (tỷ lệ nén tốt), LZ4...
- **Predicate pushdown:** nhờ thống kê min/max trong footer, engine có thể bỏ qua cả row group không thỏa điều kiện `WHERE`.
- **Hỗ trợ dữ liệu lồng nhau (nested):** struct, list, map, dùng thuật toán repetition/definition levels.
- **Schema tự mô tả:** file chứa luôn kiểu dữ liệu, không cần suy đoán như CSV.
- **Schema evolution:** thêm cột mới tương đối dễ dàng.
- **Độc lập ngôn ngữ/engine:** đọc được bằng Spark, Trino, Hive, DuckDB, Pandas, Polars, BigQuery, Snowflake, Athena...

## Ví dụ nhanh với Python

```python
import pandas as pd

df = pd.DataFrame({
    "id": [1, 2, 3],
    "ten": ["An", "Bình", "Chi"],
    "tuoi": [25, 30, 28],
})

# Ghi file
df.to_parquet("nguoi_dung.parquet", compression="zstd")

# Chỉ đọc một số cột
df2 = pd.read_parquet("nguoi_dung.parquet", columns=["ten", "tuoi"])
```

(Cần cài `pyarrow` hoặc `fastparquet`.)

## So sánh nhanh

|Tiêu chí|CSV|Avro|Parquet|
|---|---|---|---|
|Kiểu lưu|Hàng|Hàng|Cột|
|Đọc được bằng mắt|Có|Không|Không|
|Nén|Kém|Trung bình|Rất tốt|
|Phân tích (OLAP)|Chậm|Trung bình|Rất nhanh|
|Ghi từng bản ghi (streaming)|Dễ|Rất tốt|Kém|

## Khi nào nên / không nên dùng

**Nên dùng:** data lake, phân tích, báo cáo, ETL batch, lưu trữ dữ liệu lớn, machine learning với dữ liệu dạng bảng.

**Không phù hợp:**

- Ghi/cập nhật từng bản ghi liên tục (OLTP). File Parquet là **bất biến (immutable)**, muốn sửa phải ghi lại file.
- Streaming ghi từng sự kiện (Avro/Kafka phù hợp hơn).
- Dữ liệu nhỏ, cần mở và chỉnh bằng tay.
- Nhiều file quá nhỏ (_small files problem_) làm giảm hiệu năng đáng kể.

Để khắc phục hạn chế cập nhật, người ta xây các **table format** như **Delta Lake, Apache Iceberg, Apache Hudi** trên nền Parquet, bổ sung ACID transaction, update/delete và time travel.

## Mẹo thực tế

- Kích thước file lý tưởng khoảng 128MB–1GB.
- **Partition** theo cột hay lọc (ví dụ ngày tháng) để giảm lượng dữ liệu phải quét.
- Sắp xếp dữ liệu theo cột hay lọc để thống kê min/max hiệu quả hơn.