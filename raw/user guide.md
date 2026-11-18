# Hướng dẫn cho kỹ sư CK: truy cập account khách hàng

Tài liệu này dành cho người được cấp quyền truy cập account khách hàng qua mô hình hub-and-spoke.
Có hai cách dùng, cả hai đều bắt đầu từ **access key của hub** (Bước 1 và 2):

- Phần A: lấy credentials để **deploy bằng CLI/Terraform** vào account khách.
- Phần B: vào **AWS Console** của account khách.

## Cách hoạt động

```
Bạn  ->  AWS access portal  ->  account HUB (Service Account)  ->  assume role CK-<Tier>  ->  account KHÁCH
         đăng nhập SSO          role CK-<code>-<Tier>              (kèm SourceIdentity)
```

- Bạn **không** đăng nhập thẳng vào account khách. Đăng nhập portal chỉ cho bạn phiên ở **hub**, nên `aws sts get-caller-identity` ở bước này luôn ra Account ID của hub.
- Để làm việc trong account khách, bạn phải chuyển sang role `CK-<Tier>` ở account khách. Script `ck-access.sh` làm việc này.
- Mọi hành động của bạn trong account khách được ghi vào CloudTrail kèm tên của bạn (`SourceIdentity`).

Quyền của bạn phụ thuộc vào group bạn thuộc về:

| Group (Identity Center) | Role ở hub | Role ở account khách | Quyền |
|---|---|---|---|
| `grp-<code>-readonly` | `CK-<code>-ReadOnly` | `CK-ReadOnly` | Chỉ đọc |
| `grp-<code>-ops` | `CK-<code>-Ops` | `CK-Ops` | PowerUserAccess |
| `grp-<code>-admin` | `CK-<code>-Admin` | `CK-Admin` | AdministratorAccess |

Khách hàng hiện có (cập nhật theo `customers.tfvars`):

| Code | Account ID khách | Tier |
|---|---|---|
| `unicharm` | `162168468537` | Admin |

## Chuẩn bị (làm một lần)

- Bạn đã được thêm vào group tương ứng. Nếu chưa, nhờ người quản lý `customers.tfvars` thêm bạn.
- Máy có AWS CLI v2 và `python3`. Kiểm tra: `aws --version`, `python3 --version`.
- Có file `ck-access.sh` (thư mục repo). Cấp quyền chạy: `chmod +x ck-access.sh`.
- Biết **URL của AWS access portal** của CK (dạng `https://<tên>.awsapps.com/start`).

## Bước 1: Lấy access key của hub

1. Mở AWS access portal, đăng nhập bằng tài khoản của bạn (có MFA).
2. Chọn account **Service Account (hub)**, ở dòng role `CK-<code>-<Tier>` (ví dụ `CK-unicharm-Admin`) bấm **Access keys**.
3. Ở **Option 1 (Set AWS environment variables)**, sao chép khối lệnh `export` cho Linux/macOS và dán vào terminal.

Access key này **hết hạn sau 1 giờ**. Khi gặp lỗi `ExpiredToken`, quay lại bước này lấy key mới.

## Bước 2: Kiểm tra bạn đang ở hub

```bash
aws sts get-caller-identity
```

Kết quả phải có `AWSReservedSSO_CK-<code>-<Tier>_...` trong `Arn` và Account ID của hub. Nếu terminal có sẵn `AWS_ACCESS_KEY_ID` của account khác, chạy trước:

```bash
unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN AWS_PROFILE
```

rồi dán lại key ở Bước 1. Biến môi trường luôn được ưu tiên hơn `--profile`.

---

## Phần A: Deploy vào account khách bằng CLI hoặc Terraform

### A1. Lấy credentials của account khách

```bash
eval "$(./ck-access.sh env <customer_account_id> <Tier>)"
```

Ví dụ với unicharm:

```bash
eval "$(./ck-access.sh env 162168468537 Admin)"
```

### A2. Kiểm tra trước khi làm bất cứ điều gì

```bash
aws sts get-caller-identity
```

Phải ra **Account ID của khách** và `assumed-role/CK-<Tier>/<tên của bạn>`. Nếu vẫn ra Account ID của hub, bước A1 đã lỗi: **không deploy**, đọc thông báo lỗi của script.

### A3. Deploy

Đặt region trước, rồi dùng công cụ như bình thường:

```bash
export AWS_REGION=ap-southeast-1
```

CloudFormation:

```bash
aws cloudformation deploy \
  --stack-name <ten-stack> \
  --template-file <file.yaml> \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameter-overrides Key=Value

aws cloudformation describe-stacks --stack-name <ten-stack> --query 'Stacks[0].StackStatus'
```

Terraform (credentials được đọc từ biến môi trường):

```bash
terraform init
terraform plan -out=tfplan      # đọc kỹ plan, đảm bảo đúng account
terraform apply tfplan
```

Nếu Terraform dùng `provider "aws" { profile = ... }` thì biến môi trường bị bỏ qua. Hãy bỏ `profile` hoặc bỏ `AWS_PROFILE`.

### A4. Kết thúc

```bash
unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN
```

### Lưu ý khi deploy

- Phiên của account khách kéo dài **tối đa 1 giờ** (giới hạn của role chaining, không thể kéo dài). Với việc deploy dài hơn, chạy lại Bước 1 rồi A1.
- Role `CK-<Tier>` chỉ có quyền của tier đó. Với `ReadOnly` thì không deploy được.
- Terraform apply kéo dài hơn 1 giờ sẽ bị hết phiên giữa chừng và có thể để lại trạng thái dở dang. Với thay đổi lớn, chia thành nhiều lần apply nhỏ.

---

## Phần B: Dùng AWS Console của account khách

Không dùng chức năng **Switch role** trên console. Thay vào đó, dùng link đăng nhập do script tạo ra.

### B1. Tạo và mở link

Sau Bước 1 và 2:

```bash
./ck-access.sh console <customer_account_id> <Tier> --open
```

Ví dụ:

```bash
./ck-access.sh console 162168468537 ReadOnly --open
```

- Trên WSL/Windows: script mở **Edge InPrivate**.
- Nếu không mở được, bỏ `--open`. Script in link ra terminal. Mở **cửa sổ ẩn danh** của trình duyệt và dán toàn bộ link. Link rất dài (khoảng 1600 ký tự), hãy sao chép đủ cả dòng, hoặc dùng `--open`.
- Console mở ở region `ap-southeast-1`. Muốn region khác: thêm `--region <region>`.

### B2. Kiểm tra

Bấm tên ở góc trên phải. Phải thấy **Account ID của khách** và role `CK-<Tier>/<tên của bạn>`. Nếu thấy account khác, đóng cửa sổ và làm lại.

### B3. Lưu ý khi dùng console

- Link chỉ dùng **một lần** và hết hạn sau **15 phút** nếu chưa mở. Cần link mới thì chạy lại lệnh (có thể cần lấy key mới ở Bước 1 nếu key hub đã hết hạn).
- Phiên console tồn tại **tối đa 1 giờ**. Hết giờ, console yêu cầu đăng nhập lại: chạy lại B1.
- **Luôn dùng cửa sổ ẩn danh.** Link sẽ ghi đè phiên AWS bạn đang đăng nhập trong cùng trình duyệt.
- Mỗi khách một cửa sổ ẩn danh riêng. Không dùng chung một cửa sổ cho hai khách.

---

## Quy tắc an toàn

- Link console và access key tương đương mật khẩu của account khách. **Không gửi** qua chat/email, không chụp màn hình, không lưu vào file, không commit vào git.
- Chỉ dùng cho công việc được giao cho khách hàng đó. Mọi hành động đều ghi lại tên bạn trong CloudTrail của khách (`SourceIdentity`).
- Không dùng `--user` để điền tên người khác vào `SourceIdentity`. Mặc định script dùng tên SSO của bạn.
- Dùng role thấp nhất đủ cho việc cần làm (`ReadOnly` trước, `Ops`, rồi `Admin`).

## Xử lý lỗi

| Hiện tượng | Nguyên nhân và cách xử lý |
|---|---|
| `LOI: chua co credentials hop le hoac da het han` hoặc `ExpiredToken` | Key hub hết hạn (1 giờ). Làm lại Bước 1. |
| `LOI: phien hien tai khong phai permission set CK-<code>-<Tier>` | Terminal đang dùng key của role khác (ví dụ AdministratorAccess, hoặc của account khác). `unset` các biến `AWS_*`, làm lại Bước 1 với đúng role `CK-<code>-<Tier>`. |
| `AccessDenied ... sts:AssumeRole` khi chạy script | (a) Bạn dùng role `CK-<code>-<Tier>` của khách A để vào khách B, hoặc tier không khớp. (b) Bạn chưa thuộc group `grp-<code>-<tier>`. (c) Khách chưa deploy stack CloudFormation. (d) `account_id` hoặc `Tier` sai. |
| `get-caller-identity` ở A2 vẫn ra account hub | Script đã lỗi (đọc thông báo) hoặc bạn chạy `aws` trong terminal khác. Không deploy. |
| `Requires capabilities : [CAPABILITY_NAMED_IAM]` | Thêm `--capabilities CAPABILITY_NAMED_IAM`. |
| `AccessDenied` khi deploy trong account khách | Tier không đủ quyền (ví dụ `ReadOnly`), hoặc permissions boundary của khách chặn. |
| Link console báo lỗi hoặc không đăng nhập được | Link đã dùng hoặc quá 15 phút. Chạy lại B1. |
| Nút **Switch role** không hiện hoặc báo `The selected session doesn't have permission to switch to that role` | Đây là hạn chế đã biết khi dùng phiên Identity Center. Dùng Phần B. |
| Console tự đăng xuất sau khoảng 1 giờ | Bình thường (giới hạn role chaining). Chạy lại B1. |
