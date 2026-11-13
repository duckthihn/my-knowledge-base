## Là gì?

AWS Lake Formation là dịch vụ giúp bạn xây dựng, bảo mật và quản lý **data lake** (kho dữ liệu thô tập trung) trên AWS, thường lưu trong Amazon S3. Giá trị lớn nhất của nó là **quản lý quyền truy cập dữ liệu tập trung** ở mức chi tiết, thay vì phải tự viết nhiều IAM policy và S3 bucket policy phức tạp.

## Các thành phần chính

- **Data Catalog**: Lake Formation dùng chung AWS Glue Data Catalog để lưu metadata (database, table, schema).
- **Ingestion (nạp dữ liệu)**: Blueprints và workflow (dựa trên Glue) giúp đưa dữ liệu từ database (RDS, MySQL, PostgreSQL...) hoặc log (CloudTrail, ELB...) vào S3.
- **Cơ chế phân quyền**: Cấp quyền kiểu SQL (`GRANT`/`REVOKE`) trên database, table, column cho IAM user/role, nhóm hoặc tài khoản AWS khác.
- **Governed tables và table format**: Hỗ trợ các định dạng như Apache Iceberg, giúp có giao dịch ACID trên S3.

## Các tính năng bảo mật nổi bật

1. **Column-level, row-level và cell-level security**: Ví dụ, nhóm Marketing chỉ thấy cột không chứa thông tin cá nhân, hoặc chỉ thấy các dòng thuộc khu vực của họ (dùng _data filters_).
2. **LF-Tags (Tag-based access control)**: Gắn tag như `sensitivity=high` hoặc `department=finance` lên tài nguyên, rồi cấp quyền theo tag. Cách này dễ mở rộng hơn cấp quyền từng bảng.
3. **Cross-account sharing**: Chia sẻ dữ liệu giữa các tài khoản AWS hoặc trong AWS Organizations mà không cần sao chép dữ liệu.
4. **Audit**: Tích hợp CloudTrail để theo dõi ai truy cập dữ liệu nào.

## Các dịch vụ tích hợp

Quyền do Lake Formation quản lý sẽ được áp dụng khi truy vấn qua:

- Amazon Athena
- Amazon Redshift Spectrum
- Amazon EMR
- AWS Glue
- Amazon QuickSight (một số trường hợp)

## Luồng sử dụng điển hình

1. Đăng ký vị trí S3 với Lake Formation.
2. Nạp dữ liệu và tạo bảng trong Glue Data Catalog.
3. Định nghĩa quyền (theo bảng, cột, hoặc LF-Tags).
4. Người dùng truy vấn bằng Athena/Redshift/EMR và chỉ thấy phần dữ liệu được phép.

## Chi phí

Bản thân Lake Formation **không tính phí riêng**. Bạn trả tiền cho các dịch vụ bên dưới như S3, Glue (crawler, ETL job, Data Catalog), Athena, Redshift... Vì chính sách giá có thể thay đổi, hãy kiểm tra trang giá chính thức của AWS trước khi triển khai.

## Khi nào nên dùng?

**Phù hợp**: Có nhiều nhóm/phòng ban dùng chung dữ liệu, cần kiểm soát quyền chi tiết (đặc biệt dữ liệu nhạy cảm, tuân thủ quy định), hoặc chia sẻ dữ liệu giữa nhiều tài khoản.

**Có thể không cần**: Data lake nhỏ, ít người dùng, chỉ cần phân quyền ở mức bucket hoặc prefix thì IAM thông thường là đủ.

## Một số lưu ý

- Cần hiểu mối quan hệ giữa **IAM permission** và **Lake Formation permission**, vì cả hai cùng được đánh giá. Đây là chỗ người mới dễ bị lỗi "Access Denied".
- Có hai chế độ phân quyền (IAM-only và Lake Formation), và việc chuyển đổi cần được lên kế hoạch cẩn thận với dữ liệu hiện có.
- Cross-account sharing dùng AWS RAM, nên cần cấu hình thêm ở phía tài khoản nhận.