# Công thức & nguồn dữ liệu

Mỗi thẻ trên dashboard: cách tính nghiệp vụ, công thức {BẢNG.Cột}, truy vết 6 tầng từ raw SOL_HRM đến KPI và lưu ý mức sẵn sàng.

_Nguồn: DG-0004_M6_DASHBOARD_SOL_v7.html#congthuc · 138 thẻ_

## Mục lục

- **Cấp công ty · SOL E&C** (73 thẻ)
    - Tổng quan · PG-GRP-01 (9 thẻ)
    - Doanh thu theo lĩnh vực & dự án · PG-GRP-02 (8 thẻ)
    - Quỹ lương & chi phí nhân sự · PG-GRP-09 (8 thẻ)
    - Cơ cấu nhân sự · PG-GRP-03 (6 thẻ)
    - Tuổi, thâm niên & vùng miền · PG-GRP-05 (8 thẻ)
    - Tuyển dụng · PG-GRP-06 (9 thẻ)
    - Nghỉ việc · PG-GRP-6B (13 thẻ)
    - Đào tạo · PG-GRP-07 (11 thẻ)
    - Hồ sơ cán bộ quản lý · PG-GRP-10 (1 thẻ)
- **Cấp hệ sinh thái · Tổng HST** (65 thẻ)
    - Tổng quan · PG-GRP-01 (7 thẻ)
    - Doanh thu theo lĩnh vực & dự án · PG-GRP-02 (7 thẻ)
    - Quỹ lương & chi phí nhân sự · PG-GRP-09 (7 thẻ)
    - Sơ đồ tổ chức · PG-HST-ORG (1 thẻ)
    - Cơ cấu nhân sự · PG-GRP-03 (6 thẻ)
    - Tuổi, thâm niên & vùng miền · PG-GRP-05 (8 thẻ)
    - Tuyển dụng · PG-GRP-06 (8 thẻ)
    - Nghỉ việc · PG-GRP-6B (11 thẻ)
    - Đào tạo · PG-GRP-07 (9 thẻ)
    - Hồ sơ cán bộ quản lý · PG-GRP-10 (1 thẻ)

---

## Cấp công ty · SOL E&C (73 thẻ)

### Tổng quan · PG-GRP-01 (9 thẻ)

#### Tổng nhân sự hiện tại

`PG-GRP-01-V04-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tổng nhân sự bình quân

`PG-GRP-01-V08-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_D_CALENDAR.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tuyển mới đang làm việc

`PG-GRP-01-V09-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Doanh thu thực hiện / kế hoạch năm

`PG-GRP-01-V03-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).; % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm; KPI-036 Tổng doanh thu kế hoạch năm; KPI-059 % hoàn thành kế hoạch doanh thu lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000 | KPI-059 = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033, KPI-036, KPI-059

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Năng suất lao động thực hiện / kế hoạch

`PG-GRP-01-V02-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân.; NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt.; % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch.

**Mã KPI**

KPI-026 NSLĐ / đầu người (tỷ đồng/người); KPI-056 NSLĐ kế hoạch năm (tỷ đồng/người); KPI-058 % đạt NSLĐ kế hoạch

**Công thức tính**

```
KPI-026 = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.GRP_F_ANNUAL_PLAN.HC_Avg} (trường tính trong Gold)) | KPI-056 = sum({GLD.GRP_F_ANNUAL_PLAN.Productivity_Plan}) | KPI-058 = {GLD.GRP_F_ANNUAL_PLAN.NSLD_Dau_Nguoi} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.NSLD_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG, BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_ANNUAL_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_EMPLOYEE_BALANCE, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-026, KPI-056, KPI-058

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Nhân sự trung bình trong năm

`PG-GRP-01-V06-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayNghiViec); dbo.ContractLabors(TG_Tu); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_F_EMPLOYEE_TRANSACTION; tính trong Gold: Is_Month_End
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo khối

`PG-GRP-01-V07-HST` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-012 Khối HST (VP / Công trường / Nhà máy)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-012 = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [0, 4, 9]), "Văn phòng", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 2, 8]), "Công trường", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Nhà máy", "Khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.EmployeeWorkProcesses(OrganizationId); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_DEPARTMENT; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-012

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Doanh thu và năng suất lao động theo tháng

`SOL-01-RN` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayNghiViec); dbo.ContractLabors(TG_Tu); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_F_EMPLOYEE_TRANSACTION; tính trong Gold: Is_Month_End
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE, F_PROJECT_REVENUE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Năng suất lao động toàn công ty và theo khối

`SOL-01-NK` · SOL E&C · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayNghiViec); dbo.ContractLabors(TG_Tu); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_F_EMPLOYEE_TRANSACTION; tính trong Gold: Is_Month_End
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_PROJECT_REVENUE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 (8 thẻ)

#### Kế hoạch doanh thu

`PG-GRP-02-V01-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN
6. **Thẻ v4:** KPI-036

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_ANNUAL_PLAN)

#### Doanh thu thực hiện / kế hoạch năm

`PG-GRP-02-V09-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).; % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm; KPI-036 Tổng doanh thu kế hoạch năm; KPI-059 % hoàn thành kế hoạch doanh thu lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000 | KPI-059 = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033, KPI-036, KPI-059

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu trực tiếp thực hiện / kế hoạch

`PG-GRP-02-V03-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được).; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-037 Doanh thu trực tiếp / NSC so kế hoạch; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-037 = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct}) | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-037, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu NSC thực hiện / kế hoạch

`PG-GRP-02-V08-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được).; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-037 Doanh thu trực tiếp / NSC so kế hoạch; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-037 = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct}) | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-037, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ

`SOL-02-REV` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng.; Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị.; % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực.

**Mã KPI**

KPI-032 Doanh thu thực hiện (tỷ đồng); KPI-034 Lĩnh vực kinh doanh (theo loại đơn vị); KPI-035 % doanh thu theo lĩnh vực

**Công thức tính**

```
KPI-032 = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000 | KPI-034 = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định") | KPI-035 = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_D_DEPARTMENT, GLD.GRP_D_BUSINESS_LINE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-032, KPI-034, KPI-035

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_BUSINESS_LINE

#### Doanh thu theo từng lĩnh vực kinh doanh

`PG-GRP-02-V04-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng.; Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị.; % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực.

**Mã KPI**

KPI-032 Doanh thu thực hiện (tỷ đồng); KPI-034 Lĩnh vực kinh doanh (theo loại đơn vị); KPI-035 % doanh thu theo lĩnh vực

**Công thức tính**

```
KPI-032 = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000 | KPI-034 = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định") | KPI-035 = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_D_DEPARTMENT, GLD.GRP_D_BUSINESS_LINE
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, D_PROJECT, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-032, KPI-034, KPI-035

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_BUSINESS_LINE

#### Doanh thu theo vùng miền

`PG-GRP-02-V20-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_D_BUSINESS_LINE.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_D_DEPARTMENT, GLD.GRP_D_BUSINESS_LINE
5. **Mô hình chuẩn HST (CK2):** D_PROJECT, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_BUSINESS_LINE

#### Top 10 dự án có năng suất lao động không đạt

`PG-GRP-02-V02-HST` · SOL E&C · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị.; % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất).

**Mã KPI**

KPI-062 NSLĐ theo đơn vị / dự án; KPI-063 % đạt NSLĐ kế hoạch của dự án

**Công thức tính**

```
KPI-062 = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.SOL_D_DEPARTMENT.HC_Avg_Du_An} (trường tính trong Gold)) | KPI-063 = {GLD.SOL_D_DEPARTMENT.NSLD_Du_An} (trường tính trong Gold) / maxIf({GLD.SOL_F_FINANCIAL_PLAN.Productivity_Plan}, {GLD.SOL_D_DEPARTMENT.Is_Latest_Int} (trường tính trong Gold) = 1)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, MaDuAn, Month, Year); dbo.OrganizationEmployeeNorms(NangXuatLaoDong, OrganizationId); dbo.Employees(Id, LastOrgId); dbo.CcmToHrmInformations(TenDuAn); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.OrganizationEmployeeNorms, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CcmToHrmInformations, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG, BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.BRAVO.B20Product
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONEMPLOYEENORMS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_BRAVO_B20PRODUCT và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_F_FINANCIAL_PLAN, GLD.SOL_D_DEPARTMENT, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_PROJECT; tính trong Gold: Is_Latest, Is_Month_End, Is_Month_Start, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, D_PROJECT, F_EMPLOYEE_BALANCE, F_FINANCIAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-062, KPI-063

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Quỹ lương & chi phí nhân sự · PG-GRP-09 (8 thẻ)

#### Quỹ lương thực tế / quỹ lương kế hoạch

`PG-GRP-09-V01-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm.; % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch.

**Mã KPI**

KPI-030 Quỹ lương đã thực hiện; KPI-057 Quỹ lương kế hoạch năm; KPI-075 % quỹ lương thực hiện so kế hoạch/ngân sách

**Công thức tính**

```
KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-057 = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000 | KPI-075 = periodToDateSum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {GLD.GRP_F_ANNUAL_PLAN.QL_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.OrganizationEmployeeNorms(QuyLuongKeHoach)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.OrganizationEmployeeNorms
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_ORGANIZATIONEMPLOYEENORMS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_ANNUAL_PLAN, GLD.SOL_F_FINANCIAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_EMPLOYEE_EXPENSE
6. **Thẻ v4:** KPI-030, KPI-057, KPI-075

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan, GLD.GRP_F_ANNUAL_PLAN.Plan_Year; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Quỹ lương kế hoạch / doanh thu kế hoạch

`PG-GRP-09-V02-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-057 Quỹ lương kế hoạch năm; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-057 = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD003, SD104); dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.CbXSalaries(Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.CbXSalaries; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_CBXSALARIES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN
6. **Thẻ v4:** KPI-057, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Quỹ lương thực tế / doanh thu thực tế

`PG-GRP-09-V13-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện.; Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.

**Mã KPI**

KPI-028 Tỷ lệ quỹ lương / doanh thu; KPI-030 Quỹ lương đã thực hiện; KPI-033 Doanh thu thực hiện lũy kế năm

**Công thức tính**

```
KPI-028 = (sum({GLD.SOL_F_SALARY.Total_Payroll_Fund}) + sum({GLD.SOL_F_SALARY.Company_Insurance})) / sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) | KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD003, SD104); dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.CbXSalaries(Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.CbXSalaries; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_CBXSALARIES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_EXPENSE, F_PROJECT_REVENUE, F_SALARY
6. **Thẻ v4:** KPI-028, KPI-030, KPI-033

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Tổng quỹ lương theo tháng

`PG-GRP-09-V03-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi.

**Mã KPI**

KPI-030 Quỹ lương đã thực hiện; KPI-031 Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi)

**Công thức tính**

```
KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-031 = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, SD019, SD020, SD021, SD022, SD023…); dbo.CbXSalaries(Month, Year); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CbXSalaries, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_CBXSALARIES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_ORGANIZATIONS, SLV.SOL_MNL_SALARY_COMPONENT; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_EXPENSE, GLD.SOL_D_DEPARTMENT
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_EXPENSE
6. **Thẻ v4:** KPI-030, KPI-031

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Quỹ lương lũy kế so với kế hoạch năm

`SOL-09-LK` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi.

**Mã KPI**

KPI-030 Quỹ lương đã thực hiện; KPI-031 Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi)

**Công thức tính**

```
KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-031 = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, SD019, SD020, SD021, SD022, SD023…); dbo.CbXSalaries(Month, Year); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CbXSalaries, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_CBXSALARIES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_ORGANIZATIONS, SLV.SOL_MNL_SALARY_COMPONENT; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_EXPENSE, GLD.SOL_D_DEPARTMENT
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_ANNUAL_PLAN, F_EMPLOYEE_EXPENSE
6. **Thẻ v4:** KPI-030, KPI-031

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu thu nhập: lương – phụ cấp – thưởng

`PG-GRP-09-V08-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi.; Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa.

**Mã KPI**

KPI-031 Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi); KPI-030 Quỹ lương đã thực hiện; KPI-029 Tổng thu nhập

**Công thức tính**

```
KPI-031 = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương") | KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-029 = sum({GLD.SOL_F_SALARY.Total_Income})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, SD019, SD020, SD021, SD022, SD023…); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_SALARY_COMPONENT, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_EXPENSE, GLD.SOL_F_SALARY
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_SALARY_COMPONENT, F_EMPLOYEE_EXPENSE, F_SALARY
6. **Thẻ v4:** KPI-031, KPI-030, KPI-029

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Lương bình quân theo ngạch chức danh

`PG-GRP-09-V04-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code.; Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ.

**Mã KPI**

KPI-072 Lương bình quân theo cấp bậc; KPI-027 Lương bình quân / người

**Công thức tính**

```
KPI-072 = sum({GLD.SOL_F_SALARY.Actual_Salary}) / distinct_count(concat(toString({GLD.SOL_F_SALARY.Employee_Key}), toString({GLD.SOL_F_SALARY.Month_Key}))) | KPI-027 = sum({GLD.SOL_F_SALARY.Total_Income}) / avg({GLD.SOL_F_EMPLOYEE_BALANCE.HC_Avg} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, EmployeeId, GloneId, OrganizationId, SD067); dbo.CbXSalaries(Month, Year); dbo.Glones(Code); dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CbXSalaries, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_CBXSALARIES, SLV.SOL_HRM_GLONES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_EMPLOYEE_BALANCE; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, F_EMPLOYEE_BALANCE, F_SALARY
6. **Thẻ v4:** KPI-072, KPI-027

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự có lương ngoài khung theo ngạch chức danh

`PG-GRP-09-V05-HST` · SOL E&C · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc.

**Mã KPI**

KPI-074 Số NS có lương ngoài khung

**Công thức tính**

```
KPI-074 = distinct_countIf({GLD.SOL_F_SALARY.Employee_Key}, {GLD.SOL_F_SALARY.Actual_Salary} < {GLD.GRP_D_SALARY_BAND.Band_Min} OR {GLD.SOL_F_SALARY.Actual_Salary} > {GLD.GRP_D_SALARY_BAND.Band_Max})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, GloneId, EmployeeId); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.Glones
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.GRP_D_SALARY_BAND
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, D_SALARY_BAND, F_SALARY
6. **Thẻ v4:** KPI-074

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_D_SALARY_BAND.Band_Min, GLD.GRP_D_SALARY_BAND.Band_Max, GLD.GRP_D_SALARY_BAND.Level_Code; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_SALARY_BAND

### Cơ cấu nhân sự · PG-GRP-03 (6 thẻ)

#### Cơ cấu nhân sự theo cấp bậc

`PG-GRP-03-V10-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-015 Nhóm vị trí HST (theo tên chức danh)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-015 = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.Glones(Code); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES, SLV.SOL_MNL_LEVEL, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-015

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo chức danh

`PG-GRP-03-V11-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-015 Nhóm vị trí HST (theo tên chức danh)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-015 = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.Glones(Code); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES, SLV.SOL_MNL_LEVEL, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-015

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Headcount theo phòng ban khối văn phòng

`PG-GRP-03-V01-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự.; Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu).

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-039 Định biên kế hoạch; KPI-066 Thừa / thiếu so định biên

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-039 = sum({GLD.SOL_F_HEADCOUNT_PLAN.Headcount_Plan}) | KPI-066 = {GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold) - {GLD.SOL_D_DEPARTMENT.Dinh_Bien_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.EmployeeWorkProcesses(OrganizationId); dbo.Organizations(Name, ParentId); dbo.ManeuveEmpPlanDetails(PlanPositionId, SoLuong, Month, Year); dbo.ManeuveEmpPlanPositions(PlanId); dbo.ManeuveEmpPlans(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.Organizations, BRZ.SOL.HRM.ManeuveEmpPlanDetails, BRZ.SOL.HRM.ManeuveEmpPlanPositions, BRZ.SOL.HRM.ManeuveEmpPlans; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_ORGANIZATIONS, SLV.SOL_HRM_MANEUVEEMPPLANDETAILS, SLV.SOL_HRM_MANEUVEEMPPLANPOSITIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_DEPARTMENT, GLD.SOL_F_HEADCOUNT_PLAN; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_HEADCOUNT_PLAN
6. **Thẻ v4:** KPI-001, KPI-039, KPI-066

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo trường tốt nghiệp

`PG-GRP-03-V12-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.EmployeeDetails(NoiDaoTao); dbo.Schools(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.Schools; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_TOP_SCHOOL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_SCHOOLS, SLV.SOL_MNL_TOP_SCHOOL; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo ngành nghề chuyên môn

`PG-GRP-03-V05-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-014 Nhóm ngành HST (theo chuyên ngành)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-014 = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, ChuyenNganh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-014

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo học vị

`PG-GRP-03-V09-HST` · SOL E&C · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, TrinhDoDaoTao)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_EDUCATION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_EDUCATION; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Tuổi, thâm niên & vùng miền · PG-GRP-05 (8 thẻ)

#### Cơ cấu nhân sự theo nhóm tuổi

`PG-GRP-05-V03-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-007 Tuổi bình quân

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-007 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, NgaySinh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_BINS, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_AGING_GROUP, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-007

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo thế hệ

`PG-GRP-05-V20-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-007 Tuổi bình quân

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-007 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, NgaySinh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_BINS, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_AGING_GROUP, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-007

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo thâm niên

`PG-GRP-05-V06-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt.; Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-008 Thâm niên bình quân (năm); KPI-013 Nhóm thâm niên HST

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-008 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-013 = ifelse({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 1, "< 1 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 3, "1 - 3 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 5, "3 - 5 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 7, "5 - 7 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 10, "7 - 10 năm", ">= 10 năm")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayVaoCongTyChinhThuc, ThoiGianKhongTinhThamNien, NgayVaoGroup); dbo.ContractLabors(TG_Tu)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_MNL_BINS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_SENIORITY, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-008, KPI-013

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo kinh nghiệm

`PG-GRP-05-V09-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount.; Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-071 Số năm kinh nghiệm bình quân; KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-071 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Experience_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy); dbo.EmployeeDetails(NamBatDauDiLam); dbo.EmployeeExperiences(ThoiGian_Tu); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.EmployeeExperiences, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_EMPLOYEEEXPERIENCES, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-071, KPI-001

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nam – nữ

`PG-GRP-05-V02-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-068 Tỷ lệ nữ; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-068 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_D_EMPLOYEE.Gender} = "Nữ") / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, LastOrgId); dbo.OptionValues(Name); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_OPTIONVALUES, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_MNL_LEVEL; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-068, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Công tác trái miền (chỉ tính công trường)

`PG-GRP-05-V08-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0.; Tỷ lệ trái vùng miền: K09 chia K01.

**Mã KPI**

KPI-009 Số NS trái vùng miền; KPI-010 Tỷ lệ trái vùng miền

**Công thức tính**

```
KPI-009 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Out_Of_Region} = 1) | KPI-010 = {GLD.SOL_D_EMPLOYEE.HC_Trai_Vung} (trường tính trong Gold) / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, DiemGoc); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-009, KPI-010

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự tuyển dụng theo vùng miền

`PG-GRP-05-V07-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_REGION; tính trong Gold: Is_Month_End, Snapshot_Date, Region_Name
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự làm việc theo vùng miền

`PG-GRP-05-V10-HST` · SOL E&C · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_REGION; tính trong Gold: Is_Month_End, Snapshot_Date, Region_Name
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Tuyển dụng · PG-GRP-06 (9 thẻ)

#### Tuyển mới thực hiện / kế hoạch

`PG-GRP-06-V20-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-106 % hoàn thành kế hoạch tuyển

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-106 = {GLD.SOL_D_CALENDAR.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT_PLAN
6. **Thẻ v4:** KPI-018, KPI-106

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tỷ lệ chuyển đổi nhận việc

`PG-GRP-06-V22-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc.

**Mã KPI**

KPI-107 Tỷ lệ chuyển đổi phễu & thời gian tuyển TB

**Công thức tính**

```
KPI-107 = distinct_count({GLD.GRP_F_RECRUITMENT.Candidate_Key}) theo {GLD.GRP_F_RECRUITMENT.Stage}; avg(dateDiff({GLD.GRP_F_RECRUITMENT.Apply_Date}, {GLD.GRP_F_RECRUITMENT.Onboard_Date}, "DD"))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_RECRUITMENT
6. **Thẻ v4:** KPI-107

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tỷ lệ vượt qua thử việc

`PG-GRP-06-V23-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc.

**Mã KPI**

KPI-095 Tỷ lệ vượt qua thử việc

**Công thức tính**

```
KPI-095 = countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Probation_Result} = "Đạt") / countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Is_Probation} = 1 AND isNotNull({GLD.GRP_F_CONTRACT.Probation_Result}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_CONTRACT
6. **Thẻ v4:** KPI-095

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Biến động ròng theo tháng

`PG-GRP-06-V01-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Số nghỉ việc: Số lượt nghỉ việc trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-020 Số nghỉ việc

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018, KPI-020

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Phễu tuyển dụng

`SOL-06-FN` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Số nghỉ việc: Số lượt nghỉ việc trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-020 Số nghỉ việc

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** F_RECRUITMENT
6. **Thẻ v4:** KPI-018, KPI-020

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tuyển mới so với kế hoạch tuyển dụng

`PG-GRP-06-V02-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-106 % hoàn thành kế hoạch tuyển

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-106 = {GLD.GRP_F_RECRUITMENT_PLAN.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu, OrganizationId); dbo.EmployeeWorkProcesses(StartDate, OrganizationId); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc, OrganizationId); dbo.TerminationDecisions(NgayHieuLuc, OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.GRP_F_RECRUITMENT_PLAN
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT_PLAN
6. **Thẻ v4:** KPI-018, KPI-106

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT_PLAN

#### Cơ cấu nguồn tuyển dụng

`PG-GRP-06-V19-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-108 Chi phí tuyển / người theo nguồn

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-108 = sum({GLD.GRP_F_RECRUITMENT.Recruitment_Cost}) / sum({GLD.GRP_F_RECRUITMENT.Is_Hired})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.EmployeeDetails(NoiTuyenDung); dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE, GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.GRP_F_RECRUITMENT
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT
6. **Thẻ v4:** KPI-018, KPI-108

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT.Source_Code, GLD.GRP_F_RECRUITMENT.Recruitment_Cost, GLD.GRP_F_RECRUITMENT.Is_Hired; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT

#### Tuyển mới theo ngành nghề chuyên môn

`PG-GRP-06-V14-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-014 Nhóm ngành HST (theo chuyên ngành)

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-014 = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec, ChuyenNganh); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018, KPI-014

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất

`PG-GRP-06-V17-HST` · SOL E&C · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc); dbo.EmployeeDetails(ThuongTru_Tinh, ThuongTru_Tinh_Old)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Nghỉ việc · PG-GRP-6B (13 thẻ)

#### Tỷ lệ nghỉ việc lũy kế

`PG-GRP-06-V21-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ.; Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt.

**Mã KPI**

KPI-025 Tỷ lệ nghỉ việc (turnover); KPI-021 Số nghỉ việc lũy kế năm

**Công thức tính**

```
KPI-025 = sum({GLD.SOL_D_CALENDAR.Terminate_Cnt} (trường tính trong Gold)) / avg({GLD.SOL_D_CALENDAR.HC_Avg} (trường tính trong Gold)) | KPI-021 = periodToDateSum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Effective_Date}, YEAR, ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-025, KPI-021

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc tự nguyện so với không tự nguyện

`PG-GRP-06-V05-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0.; Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc.

**Mã KPI**

KPI-022 Số nghỉ việc tự nguyện; KPI-065 Tỷ lệ nghỉ việc tự nguyện

**Công thức tính**

```
KPI-022 = sumIf({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Voluntary} = "Active") | KPI-065 = {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Tu_Nguyen} (trường tính trong Gold) / {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Viec} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, RequestId); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_VOLUNTARY
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_MNL_VOLUNTARY, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-022, KPI-065

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự sắp nghỉ việc

`PG-GRP-06-V24-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng.

**Mã KPI**

KPI-109 Số NS sẽ nghỉ trong 1–3 tháng tới

**Công thức tính**

```
KPI-109 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_RESIGNATION_REQUEST
6. **Thẻ v4:** KPI-109

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo tháng

`SOL-6B-TM` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.ContractLabors(TG_Tu); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo nhóm thâm niên

`PG-GRP-06-V07-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.ContractLabors(TG_Tu); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo vùng miền quê quán

`PG-GRP-06-V16-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, OrganizationId); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu, OrganizationId); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate, OrganizationId); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc, OrganizationId); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 4 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_DEPARTMENT, GLD.SOL_D_REGION; tính trong Gold: Region_Name
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới

`PG-GRP-06-V13-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm.

**Mã KPI**

KPI-094 Số NS đến tuổi nghỉ hưu trong 1–3–5 năm

**Công thức tính**

```
KPI-094 = distinct_countIf({GLD.SOL_D_EMPLOYEE.Employee_Key}, {GLD.SOL_D_EMPLOYEE.Current_Status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {GLD.SOL_D_EMPLOYEE.DOB}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgaySinh, GioiTinh, TrangThaiCongViec, LastOrgId, Id); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_EMPLOYEE
6. **Thẻ v4:** KPI-094

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo phòng ban – Khối Văn phòng

`SOL-6B-PB` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.ContractLabors(TG_Tu); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo dự án – Khối Công trường

`SOL-6B-DA` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.ContractLabors(TG_Tu); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_PROJECT, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nguyên nhân nghỉ việc

`PG-GRP-06-V06-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, LyDo); dbo.OptionValues(Name); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.OptionValues, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_OPTIONVALUES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất

`PG-GRP-06-V15-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.EmployeeDetails(NoiDaoTao); dbo.Schools(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 3 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất

`PG-GRP-06-V18-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.

**Mã KPI**

KPI-020 Số nghỉ việc

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.EmployeeDetails(ThuongTru_Tinh, ThuongTru_Tinh_Old)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số lượng nhân sự sắp nghỉ việc

`PG-GRP-06-V12-HST` · SOL E&C · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng.

**Mã KPI**

KPI-109 Số NS sẽ nghỉ trong 1–3 tháng tới

**Công thức tính**

```
KPI-109 = distinct_countIf({GLD.GRP_F_RESIGNATION_REQUEST.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_RESIGNATION_REQUEST
5. **Mô hình chuẩn HST (CK2):** F_RESIGNATION_REQUEST
6. **Thẻ v4:** KPI-109

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_RESIGNATION_REQUEST)

### Đào tạo · PG-GRP-07 (11 thẻ)

#### Số giờ đào tạo bình quân đầu người

`PG-GRP-07-V01-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân.; % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount).

**Mã KPI**

KPI-080 Giờ đào tạo bình quân / người; KPI-081 % giờ đào tạo so kế hoạch / định mức

**Công thức tính**

```
KPI-080 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.GRP_F_TRAINING_PLAN.HC_Avg} (trường tính trong Gold)) | KPI-081 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / sum({GLD.GRP_F_TRAINING_PLAN.Planned_Hours})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(ThoiLuong, NgayKhaiGiang); dbo.TrainingStageEmployees(OrganizationId); dbo.Employees(Id); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_TRAINING_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_TRAINING, F_TRAINING_PLAN
6. **Thẻ v4:** KPI-080, KPI-081

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_PLAN

#### Chi phí đào tạo bình quân đầu người

`PG-GRP-07-V10-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0).; Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo.

**Mã KPI**

KPI-044 Chi phí đào tạo; KPI-084 Chi phí đào tạo / người

**Công thức tính**

```
KPI-044 = sum({GLD.SOL_F_TRAINING.Training_Cost_Alloc}) | KPI-084 = {GLD.GRP_F_TRAINING_PLAN.Chi_Phi_Dao_Tao} (trường tính trong Gold) / {GLD.GRP_F_TRAINING_PLAN.So_NS_Dao_Tao} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(ThoiLuong, NgayKhaiGiang); dbo.TrainingStageEmployees(OrganizationId); dbo.Employees(Id); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_TRAINING_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_TRAINING
6. **Thẻ v4:** KPI-044, KPI-084

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_PLAN

#### Kế hoạch đào tạo Hệ sinh thái

`PG-GRP-07-V20-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_TRAINING_PROGRAM_PLAN
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số giờ đào tạo khối văn phòng và khối công trường

`PG-GRP-07-V11-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia.; Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân.

**Mã KPI**

KPI-040 Số giờ đào tạo; KPI-080 Giờ đào tạo bình quân / người

**Công thức tính**

```
KPI-040 = sum({GLD.SOL_F_TRAINING.Training_Hours}) | KPI-080 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.SOL_D_TRAINING_COURSE.HC_Avg} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(EmployeeId, TinhTrang, TrainingStageId); dbo.TrainingStages(TenChuongTrinh)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_TRAINING
6. **Thẻ v4:** KPI-040, KPI-080

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 giảng viên nội bộ đứng lớp theo quý

`PG-GRP-07-V09-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5).

**Mã KPI**

KPI-111 Số lớp / giờ đứng lớp của giảng viên nội bộ

**Công thức tính**

```
KPI-111 = distinct_countIf({GLD.GRP_F_TRAINING_DELIVERY.Class_Key}, {GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor} = 1); sum({GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(NgayKhaiGiang)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_TRAINING_DELIVERY, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING_DELIVERY
6. **Thẻ v4:** KPI-111

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_DELIVERY.Instructor_Employee_Key, GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours, GLD.GRP_F_TRAINING_DELIVERY.Class_Key, GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_DELIVERY

#### Tần suất tổ chức các chương trình đào tạo trong năm

`PG-GRP-07-V04-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình).

**Mã KPI**

KPI-042 Số lớp đào tạo

**Công thức tính**

```
KPI-042 = distinct_count({GLD.SOL_F_TRAINING.Class_Key})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING
6. **Thẻ v4:** KPI-042

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số nhân sự tham gia đào tạo theo nhóm chương trình

`PG-GRP-07-V06-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp.; Lượt học viên: Số lượt tham gia có điểm danh.

**Mã KPI**

KPI-043 Số nhân sự tham gia đào tạo; KPI-083 Lượt học viên

**Công thức tính**

```
KPI-043 = distinct_countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1) | KPI-083 = countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(EmployeeId, DiemDanh_TrangThai, TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING
6. **Thẻ v4:** KPI-043, KPI-083

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số nhân sự đang được kèm cặp theo chức danh

`PG-GRP-07-V12-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** chưa khai báo
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_SUCCESSION_PLAN
6. **Thẻ v4:** KPI-001

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM

#### Nhân sự chưa có chứng chỉ tương ứng với chức danh

`PG-GRP-07-V03-HST` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh.

**Mã KPI**

KPI-085 Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh

**Công thức tính**

```
KPI-085 = distinct_countIf({GLD.GRP_F_CERTIFICATE.Employee_Key}, {GLD.GRP_F_CERTIFICATE.Du_CC_Bat_Buoc} (trường tính trong Gold) = 1) / distinct_count({GLD.GRP_F_CERTIFICATE.Employee_Key})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_CERTIFICATE
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_CERTIFICATE
6. **Thẻ v4:** KPI-085

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_CERTIFICATE)

#### Giờ đào tạo theo phòng ban khối văn phòng

`SOL-07-PB` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Giờ đào tạo lũy kế năm của từng phòng ban, kèm giờ / người

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_TRAINING
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Giờ đào tạo theo dự án / công trường

`SOL-07-DA` · SOL E&C · Đào tạo

**Cách tính nghiệp vụ**

Giờ đào tạo lũy kế năm của từng dự án, kèm giờ / người

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_PROJECT, F_EMPLOYEE_BALANCE, F_TRAINING
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Hồ sơ cán bộ quản lý · PG-GRP-10 (1 thẻ)

#### Hồ sơ năng lực cán bộ quản lý

`PG-GRP-10-V01-HST` · SOL E&C · Hồ sơ cán bộ quản lý

**Cách tính nghiệp vụ**

Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_D_EMPLOYEE.Employee_Code}, {GLD.SOL_D_EMPLOYEE.Employee_Name}, {GLD.SOL_D_EMPLOYEE.DOB}, {GLD.SOL_D_EMPLOYEE.Seniority_Start_Date}, {GLD.SOL_D_JOBTITLE.JobTitle_Name}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(StaffCode, FullName, NgaySinh, NgayVaoCongTy, NgayVaoCongTyChinhThuc, ThoiGianKhongTinhThamNien); dbo.ContractLabors(TG_Tu); dbo.JobTitles(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.JobTitles
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_JOBTITLES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE, GLD.SOL_D_JOBTITLE, GLD.GRP_F_MANAGER_ASSIGNMENT
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_JOBTITLE, F_MANAGER_ASSIGNMENT, F_SUCCESSION_PLAN
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned, GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_MANAGER_ASSIGNMENT

---

## Cấp hệ sinh thái · Tổng HST (65 thẻ)

### Tổng quan · PG-GRP-01 (7 thẻ)

#### Tổng nhân sự hiện tại

`PG-GRP-01-V04-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tổng nhân sự bình quân

`PG-GRP-01-V08-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_D_CALENDAR.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tuyển mới đang làm việc

`PG-GRP-01-V09-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Doanh thu thực hiện / kế hoạch năm

`PG-GRP-01-V03-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).; % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm; KPI-036 Tổng doanh thu kế hoạch năm; KPI-059 % hoàn thành kế hoạch doanh thu lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000 | KPI-059 = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033, KPI-036, KPI-059

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Năng suất lao động thực hiện / kế hoạch

`PG-GRP-01-V02-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân.; NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt.; % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch.

**Mã KPI**

KPI-026 NSLĐ / đầu người (tỷ đồng/người); KPI-056 NSLĐ kế hoạch năm (tỷ đồng/người); KPI-058 % đạt NSLĐ kế hoạch

**Công thức tính**

```
KPI-026 = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.GRP_F_ANNUAL_PLAN.HC_Avg} (trường tính trong Gold)) | KPI-056 = sum({GLD.GRP_F_ANNUAL_PLAN.Productivity_Plan}) | KPI-058 = {GLD.GRP_F_ANNUAL_PLAN.NSLD_Dau_Nguoi} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.NSLD_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG, BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_ANNUAL_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_EMPLOYEE_BALANCE, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-026, KPI-056, KPI-058

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Nhân sự trung bình trong năm

`PG-GRP-01-V06-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng.

**Mã KPI**

“Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Công thức tính**

```
“Tổng nhân sự bình quân” (mã có sẵn trong v4) = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayNghiViec); dbo.ContractLabors(TG_Tu); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_F_EMPLOYEE_TRANSACTION; tính trong Gold: Is_Month_End
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** “Tổng nhân sự bình quân” (mã có sẵn trong v4)

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo khối

`PG-GRP-01-V07-HST` · Tổng HST · Tổng quan

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-012 Khối HST (VP / Công trường / Nhà máy)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-012 = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [0, 4, 9]), "Văn phòng", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 2, 8]), "Công trường", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Nhà máy", "Khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.EmployeeWorkProcesses(OrganizationId); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_DEPARTMENT; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-012

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 (7 thẻ)

#### Kế hoạch doanh thu

`PG-GRP-02-V01-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN
6. **Thẻ v4:** KPI-036

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_ANNUAL_PLAN)

#### Doanh thu thực hiện / kế hoạch năm

`PG-GRP-02-V09-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).; % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm; KPI-036 Tổng doanh thu kế hoạch năm; KPI-059 % hoàn thành kế hoạch doanh thu lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000 | KPI-059 = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033, KPI-036, KPI-059

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu trực tiếp thực hiện / kế hoạch

`PG-GRP-02-V03-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được).; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-037 Doanh thu trực tiếp / NSC so kế hoạch; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-037 = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct}) | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-037, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu NSC thực hiện / kế hoạch

`PG-GRP-02-V08-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được).; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-037 Doanh thu trực tiếp / NSC so kế hoạch; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-037 = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct}) | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-037, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Doanh thu theo từng lĩnh vực kinh doanh

`PG-GRP-02-V04-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng.; Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị.; % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực.

**Mã KPI**

KPI-032 Doanh thu thực hiện (tỷ đồng); KPI-034 Lĩnh vực kinh doanh (theo loại đơn vị); KPI-035 % doanh thu theo lĩnh vực

**Công thức tính**

```
KPI-032 = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000 | KPI-034 = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định") | KPI-035 = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_D_DEPARTMENT, GLD.GRP_D_BUSINESS_LINE
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, D_PROJECT, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-032, KPI-034, KPI-035

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_BUSINESS_LINE

#### Doanh thu theo vùng miền

`PG-GRP-02-V20-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.

**Mã KPI**

KPI-033 Doanh thu thực hiện lũy kế năm

**Công thức tính**

```
KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_D_BUSINESS_LINE.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_D_DEPARTMENT, GLD.GRP_D_BUSINESS_LINE
5. **Mô hình chuẩn HST (CK2):** D_PROJECT, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-033

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_BUSINESS_LINE

#### Top 10 dự án có năng suất lao động không đạt

`PG-GRP-02-V02-HST` · Tổng HST · Doanh thu theo lĩnh vực & dự án

**Cách tính nghiệp vụ**

NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị.; % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất).

**Mã KPI**

KPI-062 NSLĐ theo đơn vị / dự án; KPI-063 % đạt NSLĐ kế hoạch của dự án

**Công thức tính**

```
KPI-062 = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.SOL_D_DEPARTMENT.HC_Avg_Du_An} (trường tính trong Gold)) | KPI-063 = {GLD.SOL_D_DEPARTMENT.NSLD_Du_An} (trường tính trong Gold) / maxIf({GLD.SOL_F_FINANCIAL_PLAN.Productivity_Plan}, {GLD.SOL_D_DEPARTMENT.Is_Latest_Int} (trường tính trong Gold) = 1)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CcmToHrmMonthRevenues(DT_ThucTe, MaDuAn, Month, Year); dbo.OrganizationEmployeeNorms(NangXuatLaoDong, OrganizationId); dbo.Employees(Id, LastOrgId); dbo.CcmToHrmInformations(TenDuAn); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.OrganizationEmployeeNorms, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CcmToHrmInformations, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG, BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.BRAVO.B20Product
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_ORGANIZATIONEMPLOYEENORMS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_BRAVO_B20PRODUCT và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_PROJECT_REVENUE, GLD.SOL_F_FINANCIAL_PLAN, GLD.SOL_D_DEPARTMENT, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_PROJECT; tính trong Gold: Is_Latest, Is_Month_End, Is_Month_Start, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, D_PROJECT, F_EMPLOYEE_BALANCE, F_FINANCIAL_PLAN, F_PROJECT_REVENUE
6. **Thẻ v4:** KPI-062, KPI-063

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Quỹ lương & chi phí nhân sự · PG-GRP-09 (7 thẻ)

#### Quỹ lương thực tế / quỹ lương kế hoạch

`PG-GRP-09-V01-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm.; % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch.

**Mã KPI**

KPI-030 Quỹ lương đã thực hiện; KPI-057 Quỹ lương kế hoạch năm; KPI-075 % quỹ lương thực hiện so kế hoạch/ngân sách

**Công thức tính**

```
KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-057 = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000 | KPI-075 = periodToDateSum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {GLD.GRP_F_ANNUAL_PLAN.QL_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.OrganizationEmployeeNorms(QuyLuongKeHoach)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.OrganizationEmployeeNorms
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_ORGANIZATIONEMPLOYEENORMS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_ANNUAL_PLAN, GLD.SOL_F_FINANCIAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN, F_EMPLOYEE_EXPENSE
6. **Thẻ v4:** KPI-030, KPI-057, KPI-075

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan, GLD.GRP_F_ANNUAL_PLAN.Plan_Year; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Quỹ lương kế hoạch / doanh thu kế hoạch

`PG-GRP-09-V02-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm.; Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn).

**Mã KPI**

KPI-057 Quỹ lương kế hoạch năm; KPI-036 Tổng doanh thu kế hoạch năm

**Công thức tính**

```
KPI-057 = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000 | KPI-036 = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD003, SD104); dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.CbXSalaries(Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.CbXSalaries; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_CBXSALARIES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_ANNUAL_PLAN
6. **Thẻ v4:** KPI-057, KPI-036

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Quỹ lương thực tế / doanh thu thực tế

`PG-GRP-09-V13-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện.; Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng.

**Mã KPI**

KPI-028 Tỷ lệ quỹ lương / doanh thu; KPI-030 Quỹ lương đã thực hiện; KPI-033 Doanh thu thực hiện lũy kế năm

**Công thức tính**

```
KPI-028 = (sum({GLD.SOL_F_SALARY.Total_Payroll_Fund}) + sum({GLD.SOL_F_SALARY.Company_Insurance})) / sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) | KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-033 = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD003, SD104); dbo.CcmToHrmMonthRevenues(DT_ThucTe, Month, Year); dbo.CbXSalaries(Month, Year)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CcmToHrmMonthRevenues, BRZ.SOL.HRM.CbXSalaries; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_CCMTOHRMMONTHREVENUES, SLV.SOL_HRM_CBXSALARIES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_PROJECT_REVENUE, GLD.GRP_F_ANNUAL_PLAN
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_EXPENSE, F_PROJECT_REVENUE, F_SALARY
6. **Thẻ v4:** KPI-028, KPI-030, KPI-033

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_ANNUAL_PLAN

#### Tổng quỹ lương theo tháng

`PG-GRP-09-V03-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi.

**Mã KPI**

KPI-030 Quỹ lương đã thực hiện; KPI-031 Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi)

**Công thức tính**

```
KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-031 = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, SD019, SD020, SD021, SD022, SD023…); dbo.CbXSalaries(Month, Year); dbo.Organizations(Type)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CbXSalaries, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_CBXSALARIES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_ORGANIZATIONS, SLV.SOL_MNL_SALARY_COMPONENT; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_EXPENSE, GLD.SOL_D_DEPARTMENT
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_EXPENSE
6. **Thẻ v4:** KPI-030, KPI-031

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu thu nhập: lương – phụ cấp – thưởng

`PG-GRP-09-V08-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi.; Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương).; Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa.

**Mã KPI**

KPI-031 Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi); KPI-030 Quỹ lương đã thực hiện; KPI-029 Tổng thu nhập

**Công thức tính**

```
KPI-031 = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương") | KPI-030 = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}) | KPI-029 = sum({GLD.SOL_F_SALARY.Total_Income})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, SD019, SD020, SD021, SD022, SD023…); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_MNL_SALARY_COMPONENT, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_EXPENSE, GLD.SOL_F_SALARY
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_SALARY_COMPONENT, F_EMPLOYEE_EXPENSE, F_SALARY
6. **Thẻ v4:** KPI-031, KPI-030, KPI-029

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Lương bình quân theo ngạch chức danh

`PG-GRP-09-V04-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code.; Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ.

**Mã KPI**

KPI-072 Lương bình quân theo cấp bậc; KPI-027 Lương bình quân / người

**Công thức tính**

```
KPI-072 = sum({GLD.SOL_F_SALARY.Actual_Salary}) / distinct_count(concat(toString({GLD.SOL_F_SALARY.Employee_Key}), toString({GLD.SOL_F_SALARY.Month_Key}))) | KPI-027 = sum({GLD.SOL_F_SALARY.Total_Income}) / avg({GLD.SOL_F_EMPLOYEE_BALANCE.HC_Avg} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, EmployeeId, GloneId, OrganizationId, SD067); dbo.CbXSalaries(Month, Year); dbo.Glones(Code); dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.CbXSalaries, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_CBXSALARIES, SLV.SOL_HRM_GLONES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.SOL_F_EMPLOYEE_BALANCE; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, F_EMPLOYEE_BALANCE, F_SALARY
6. **Thẻ v4:** KPI-072, KPI-027

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự có lương ngoài khung theo ngạch chức danh

`PG-GRP-09-V05-HST` · Tổng HST · Quỹ lương & chi phí nhân sự

**Cách tính nghiệp vụ**

Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc.

**Mã KPI**

KPI-074 Số NS có lương ngoài khung

**Công thức tính**

```
KPI-074 = distinct_countIf({GLD.SOL_F_SALARY.Employee_Key}, {GLD.SOL_F_SALARY.Actual_Salary} < {GLD.GRP_D_SALARY_BAND.Band_Min} OR {GLD.SOL_F_SALARY.Actual_Salary} > {GLD.GRP_D_SALARY_BAND.Band_Max})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.CbXSalaryDetails(SD018, GloneId, EmployeeId); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.CbXSalaryDetails, BRZ.SOL.HRM.Glones
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CBXSALARYDETAILS, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_SALARY, GLD.GRP_D_SALARY_BAND
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, D_SALARY_BAND, F_SALARY
6. **Thẻ v4:** KPI-074

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_D_SALARY_BAND.Band_Min, GLD.GRP_D_SALARY_BAND.Band_Max, GLD.GRP_D_SALARY_BAND.Level_Code; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_D_SALARY_BAND

### Sơ đồ tổ chức · PG-HST-ORG (1 thẻ)

`org-note` · Tổng HST · Sơ đồ tổ chức

**Cách tính nghiệp vụ**

Số trên ô = đếm số nhân sự đang làm việc (headcount) của đơn vị và các đơn vị cấp dưới tại ngày chốt, mỗi người đếm 1 lần.

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount}, {GLD.SOL_F_EMPLOYEE_BALANCE.Dept_Key}, {GLD.SOL_D_DEPARTMENT.Dept_Key}, {GLD.SOL_D_DEPARTMENT.Parent_Dept_Key}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.EmployeeWorkProcesses(OrganizationId); dbo.Organizations(Id, ParentId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_DEPARTMENT
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_MANAGER_ASSIGNMENT
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD); cột của ô sơ đồ suy luận từ ghi chú v4 (suy luận)

### Cơ cấu nhân sự · PG-GRP-03 (6 thẻ)

#### Cơ cấu nhân sự theo cấp bậc

`PG-GRP-03-V10-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-015 Nhóm vị trí HST (theo tên chức danh)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-015 = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.Glones(Code); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES, SLV.SOL_MNL_LEVEL, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, D_LEVEL, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-015

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo chức danh

`PG-GRP-03-V11-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-015 Nhóm vị trí HST (theo tên chức danh)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-015 = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.Glones(Code); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES, SLV.SOL_MNL_LEVEL, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-015

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Headcount theo phòng ban khối văn phòng

`PG-GRP-03-V01-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự.; Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu).

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-039 Định biên kế hoạch; KPI-066 Thừa / thiếu so định biên

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-039 = sum({GLD.SOL_F_HEADCOUNT_PLAN.Headcount_Plan}) | KPI-066 = {GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold) - {GLD.SOL_D_DEPARTMENT.Dinh_Bien_KH} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, LastOrgId); dbo.EmployeeWorkProcesses(OrganizationId); dbo.Organizations(Name, ParentId); dbo.ManeuveEmpPlanDetails(PlanPositionId, SoLuong, Month, Year); dbo.ManeuveEmpPlanPositions(PlanId); dbo.ManeuveEmpPlans(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.Organizations, BRZ.SOL.HRM.ManeuveEmpPlanDetails, BRZ.SOL.HRM.ManeuveEmpPlanPositions, BRZ.SOL.HRM.ManeuveEmpPlans; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_ORGANIZATIONS, SLV.SOL_HRM_MANEUVEEMPPLANDETAILS, SLV.SOL_HRM_MANEUVEEMPPLANPOSITIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_DEPARTMENT, GLD.SOL_F_HEADCOUNT_PLAN; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_HEADCOUNT_PLAN
6. **Thẻ v4:** KPI-001, KPI-039, KPI-066

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo trường tốt nghiệp

`PG-GRP-03-V12-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.EmployeeDetails(NoiDaoTao); dbo.Schools(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.Schools; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_TOP_SCHOOL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_SCHOOLS, SLV.SOL_MNL_TOP_SCHOOL; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo ngành nghề chuyên môn

`PG-GRP-03-V05-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-014 Nhóm ngành HST (theo chuyên ngành)

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-014 = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, ChuyenNganh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-014

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo học vị

`PG-GRP-03-V09-HST` · Tổng HST · Cơ cấu nhân sự

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, TrinhDoDaoTao)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_EDUCATION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_EDUCATION; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Tuổi, thâm niên & vùng miền · PG-GRP-05 (8 thẻ)

#### Cơ cấu nhân sự theo nhóm tuổi

`PG-GRP-05-V03-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-007 Tuổi bình quân

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-007 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, NgaySinh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_BINS, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_AGING_GROUP, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-007

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo thế hệ

`PG-GRP-05-V20-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.; Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn; KPI-007 Tuổi bình quân

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold)) | KPI-007 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, NgaySinh); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_BINS, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_AGING_GROUP, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006, KPI-007

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo thâm niên

`PG-GRP-05-V06-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt.; Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-008 Thâm niên bình quân (năm); KPI-013 Nhóm thâm niên HST

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-008 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-013 = ifelse({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 1, "< 1 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 3, "1 - 3 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 5, "3 - 5 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 7, "5 - 7 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 10, "7 - 10 năm", ">= 10 năm")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy, NgayVaoCongTyChinhThuc, ThoiGianKhongTinhThamNien, NgayVaoGroup); dbo.ContractLabors(TG_Tu)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_BINS
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_MNL_BINS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_SENIORITY, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-008, KPI-013

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nhân sự theo kinh nghiệm

`PG-GRP-05-V09-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount.; Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-071 Số năm kinh nghiệm bình quân; KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-071 = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Experience_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, NgayVaoCongTy); dbo.EmployeeDetails(NamBatDauDiLam); dbo.EmployeeExperiences(ThoiGian_Tu); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.EmployeeExperiences, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_EMPLOYEEEXPERIENCES, SLV.SOL_MNL_PROJECT_ORG, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-071, KPI-001

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Phân bổ nhân sự theo vùng miền

`PG-GRP-05-V07-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_REGION; tính trong Gold: Is_Month_End, Snapshot_Date, Region_Name
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số lượng nhân sự làm việc tại 3 miền

`PG-GRP-05-V10-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot}) | KPI-006 = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_REGION; tính trong Gold: Is_Month_End, Snapshot_Date, Region_Name
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-001, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Công tác trái miền (chỉ tính công trường)

`PG-GRP-05-V08-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0.; Tỷ lệ trái vùng miền: K09 chia K01.

**Mã KPI**

KPI-009 Số NS trái vùng miền; KPI-010 Tỷ lệ trái vùng miền

**Công thức tính**

```
KPI-009 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Out_Of_Region} = 1) | KPI-010 = {GLD.SOL_D_EMPLOYEE.HC_Trai_Vung} (trường tính trong Gold) / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, DiemGoc); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Organizations; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_MNL_PROVINCE_REGION, SLV.SOL_MNL_REGION, SLV.SOL_HRM_ORGANIZATIONS; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-009, KPI-010

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Cơ cấu nam – nữ

`PG-GRP-05-V02-HST` · Tổng HST · Tuổi, thâm niên & vùng miền

**Cách tính nghiệp vụ**

Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt.; % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt.

**Mã KPI**

KPI-068 Tỷ lệ nữ; KPI-006 % cơ cấu theo chiều đang chọn

**Công thức tính**

```
KPI-068 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_D_EMPLOYEE.Gender} = "Nữ") / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold) | KPI-006 = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id, GioiTinh, LastOrgId); dbo.OptionValues(Name); dbo.EmployeeWorkProcesses(OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues, BRZ.SOL.HRM.EmployeeWorkProcesses; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE, BRZ.SOL.EXCEL.MAP_LEVEL
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_OPTIONVALUES, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_MNL_LEVEL; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_EMPLOYEE, GLD.SOL_D_LEVEL; tính trong Gold: Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_EMPLOYEE, F_EMPLOYEE_BALANCE
6. **Thẻ v4:** KPI-068, KPI-006

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Tuyển dụng · PG-GRP-06 (8 thẻ)

#### Tuyển mới thực hiện / kế hoạch

`PG-GRP-06-V20-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-106 % hoàn thành kế hoạch tuyển

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-106 = {GLD.SOL_D_CALENDAR.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT_PLAN
6. **Thẻ v4:** KPI-018, KPI-106

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tỷ lệ chuyển đổi nhận việc

`PG-GRP-06-V22-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc.

**Mã KPI**

KPI-107 Tỷ lệ chuyển đổi phễu & thời gian tuyển TB

**Công thức tính**

```
KPI-107 = distinct_count({GLD.GRP_F_RECRUITMENT.Candidate_Key}) theo {GLD.GRP_F_RECRUITMENT.Stage}; avg(dateDiff({GLD.GRP_F_RECRUITMENT.Apply_Date}, {GLD.GRP_F_RECRUITMENT.Onboard_Date}, "DD"))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_RECRUITMENT
6. **Thẻ v4:** KPI-107

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tỷ lệ vượt qua thử việc

`PG-GRP-06-V23-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc.

**Mã KPI**

KPI-095 Tỷ lệ vượt qua thử việc

**Công thức tính**

```
KPI-095 = countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Probation_Result} = "Đạt") / countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Is_Probation} = 1 AND isNotNull({GLD.GRP_F_CONTRACT.Probation_Result}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_CONTRACT
6. **Thẻ v4:** KPI-095

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Biến động ròng theo tháng

`PG-GRP-06-V01-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Số nghỉ việc: Số lượt nghỉ việc trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-020 Số nghỉ việc

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018, KPI-020

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Tuyển mới so với kế hoạch tuyển dụng

`PG-GRP-06-V02-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-106 % hoàn thành kế hoạch tuyển

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-106 = {GLD.GRP_F_RECRUITMENT_PLAN.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu, OrganizationId); dbo.EmployeeWorkProcesses(StartDate, OrganizationId); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc, OrganizationId); dbo.TerminationDecisions(NgayHieuLuc, OrganizationId)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.GRP_F_RECRUITMENT_PLAN
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT_PLAN
6. **Thẻ v4:** KPI-018, KPI-106

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT_PLAN

#### Cơ cấu nguồn tuyển dụng

`PG-GRP-06-V19-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-108 Chi phí tuyển / người theo nguồn

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-108 = sum({GLD.GRP_F_RECRUITMENT.Recruitment_Cost}) / sum({GLD.GRP_F_RECRUITMENT.Is_Hired})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.EmployeeDetails(NoiTuyenDung); dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.EmployeeDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEEDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE, GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.GRP_F_RECRUITMENT
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION, F_RECRUITMENT
6. **Thẻ v4:** KPI-018, KPI-108

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT.Source_Code, GLD.GRP_F_RECRUITMENT.Recruitment_Cost, GLD.GRP_F_RECRUITMENT.Is_Hired; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_RECRUITMENT

#### Tuyển mới theo ngành nghề chuyên môn

`PG-GRP-06-V14-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.; Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành.

**Mã KPI**

KPI-018 Số tuyển mới; KPI-014 Nhóm ngành HST (theo chuyên ngành)

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire}) | KPI-014 = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec, ChuyenNganh); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018, KPI-014

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất

`PG-GRP-06-V17-HST` · Tổng HST · Tuyển dụng

**Cách tính nghiệp vụ**

Số tuyển mới: Số lượt tuyển mới trong kỳ.

**Mã KPI**

KPI-018 Số tuyển mới

**Công thức tính**

```
KPI-018 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.ContractLabors(TG_Tu); dbo.Employees(NgayVaoCongTy, NgayNghiViec); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.TerminationDecisions(NgayHieuLuc); dbo.EmployeeDetails(ThuongTru_Tinh, ThuongTru_Tinh_Old)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions, BRZ.SOL.HRM.PreferDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS, SLV.SOL_HRM_PREFERDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-018

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

### Nghỉ việc · PG-GRP-6B (11 thẻ)

#### Tỷ lệ nghỉ việc lũy kế

`PG-GRP-06-V21-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ.; Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt.

**Mã KPI**

KPI-025 Tỷ lệ nghỉ việc (turnover); KPI-021 Số nghỉ việc lũy kế năm

**Công thức tính**

```
KPI-025 = sum({GLD.SOL_D_CALENDAR.Terminate_Cnt} (trường tính trong Gold)) / avg({GLD.SOL_D_CALENDAR.HC_Avg} (trường tính trong Gold)) | KPI-021 = periodToDateSum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Effective_Date}, YEAR, ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-025, KPI-021

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc tự nguyện so với không tự nguyện

`PG-GRP-06-V05-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0.; Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc.

**Mã KPI**

KPI-022 Số nghỉ việc tự nguyện; KPI-065 Tỷ lệ nghỉ việc tự nguyện

**Công thức tính**

```
KPI-022 = sumIf({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Voluntary} = "Active") | KPI-065 = {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Tu_Nguyen} (trường tính trong Gold) / {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Viec} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, RequestId); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_VOLUNTARY
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_MNL_VOLUNTARY, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-022, KPI-065

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự sắp nghỉ việc

`PG-GRP-06-V24-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng.

**Mã KPI**

KPI-109 Số NS sẽ nghỉ trong 1–3 tháng tới

**Công thức tính**

```
KPI-109 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(Id)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_BALANCE, GLD.SOL_D_CALENDAR; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date, Month_Key
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_RESIGNATION_REQUEST
6. **Thẻ v4:** KPI-109

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo nhóm thâm niên

`PG-GRP-06-V07-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.ContractLabors(TG_Tu); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 1 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 1 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_BALANCE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo vùng miền quê quán

`PG-GRP-06-V16-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, OrganizationId); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu, OrganizationId); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate, OrganizationId); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc, OrganizationId); dbo.Organizations(CountryId, KhuVuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION, BRZ.SOL.EXCEL.MAP_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 4 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_DEPARTMENT, GLD.SOL_D_REGION; tính trong Gold: Region_Name
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới

`PG-GRP-06-V13-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm.

**Mã KPI**

KPI-094 Số NS đến tuổi nghỉ hưu trong 1–3–5 năm

**Công thức tính**

```
KPI-094 = distinct_countIf({GLD.SOL_D_EMPLOYEE.Employee_Key}, {GLD.SOL_D_EMPLOYEE.Current_Status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {GLD.SOL_D_EMPLOYEE.DOB}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgaySinh, GioiTinh, TrangThaiCongViec, LastOrgId, Id); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_EMPLOYEE
6. **Thẻ v4:** KPI-094

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nguyên nhân nghỉ việc

`PG-GRP-06-V06-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, LyDo); dbo.OptionValues(Name); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.OptionValues, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses và 2 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_OPTIONVALUES, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES và 2 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION
5. **Mô hình chuẩn HST (CK2):** F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Nghỉ việc theo khối công trường và phòng ban văn phòng

`PG-GRP-06-V08-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc, OrganizationId); dbo.CbEmployeeFluctuationDetails(OrganizationId, Ngay, TG_Tu); dbo.EmployeeWorkProcesses(OrganizationId, StartDate); dbo.PreferDecisions(OrganizationId, NgayHieuLuc); dbo.Organizations(Name); dbo.ContractLabors(TG_Tu); dbo.GloneDecisions(NgayHieuLuc)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.PreferDecisions, BRZ.SOL.HRM.Organizations và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROJECT_ORG
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_PREFERDECISIONS, SLV.SOL_HRM_ORGANIZATIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_DEPARTMENT
5. **Mô hình chuẩn HST (CK2):** D_DEPARTMENT, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất

`PG-GRP-06-V15-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.; % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ.

**Mã KPI**

KPI-020 Số nghỉ việc; KPI-023 % nghỉ việc theo nhóm (Top 5)

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}) | KPI-023 = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.EmployeeDetails(NoiDaoTao); dbo.Schools(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 3 mục khác
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020, KPI-023

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất

`PG-GRP-06-V18-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số nghỉ việc: Số lượt nghỉ việc trong kỳ.

**Mã KPI**

KPI-020 Số nghỉ việc

**Công thức tính**

```
KPI-020 = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(NgayNghiViec, NgayVaoCongTy); dbo.TerminationDecisions(NgayHieuLuc); dbo.CbEmployeeFluctuationDetails(Ngay, TG_Tu); dbo.ContractLabors(TG_Tu); dbo.EmployeeWorkProcesses(StartDate); dbo.GloneDecisions(NgayHieuLuc); dbo.PreferDecisions(NgayHieuLuc); dbo.EmployeeDetails(ThuongTru_Tinh, ThuongTru_Tinh_Old)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.TerminationDecisions, BRZ.SOL.HRM.CbEmployeeFluctuationDetails, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.EmployeeWorkProcesses, BRZ.SOL.HRM.GloneDecisions và 2 mục khác; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_PROVINCE_REGION
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_TERMINATIONDECISIONS, SLV.SOL_HRM_CBEMPLOYEEFLUCTUATIONDETAILS, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_EMPLOYEEWORKPROCESSES, SLV.SOL_HRM_GLONEDECISIONS và 3 mục khác; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_EMPLOYEE_TRANSACTION, GLD.SOL_D_EMPLOYEE
5. **Mô hình chuẩn HST (CK2):** D_EMPLOYEE, F_EMPLOYEE_TRANSACTION
6. **Thẻ v4:** KPI-020

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số lượng nhân sự sắp nghỉ việc

`PG-GRP-06-V12-HST` · Tổng HST · Nghỉ việc

**Cách tính nghiệp vụ**

Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng.

**Mã KPI**

KPI-109 Số NS sẽ nghỉ trong 1–3 tháng tới

**Công thức tính**

```
KPI-109 = distinct_countIf({GLD.GRP_F_RESIGNATION_REQUEST.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_RESIGNATION_REQUEST
5. **Mô hình chuẩn HST (CK2):** F_RESIGNATION_REQUEST
6. **Thẻ v4:** KPI-109

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_RESIGNATION_REQUEST)

### Đào tạo · PG-GRP-07 (9 thẻ)

#### Số giờ đào tạo bình quân đầu người

`PG-GRP-07-V01-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân.; % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount).

**Mã KPI**

KPI-080 Giờ đào tạo bình quân / người; KPI-081 % giờ đào tạo so kế hoạch / định mức

**Công thức tính**

```
KPI-080 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.GRP_F_TRAINING_PLAN.HC_Avg} (trường tính trong Gold)) | KPI-081 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / sum({GLD.GRP_F_TRAINING_PLAN.Planned_Hours})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(ThoiLuong, NgayKhaiGiang); dbo.TrainingStageEmployees(OrganizationId); dbo.Employees(Id); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_TRAINING_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_EMPLOYEE_BALANCE, F_TRAINING, F_TRAINING_PLAN
6. **Thẻ v4:** KPI-080, KPI-081

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_PLAN

#### Chi phí đào tạo bình quân đầu người

`PG-GRP-07-V10-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0).; Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo.

**Mã KPI**

KPI-044 Chi phí đào tạo; KPI-084 Chi phí đào tạo / người

**Công thức tính**

```
KPI-044 = sum({GLD.SOL_F_TRAINING.Training_Cost_Alloc}) | KPI-084 = {GLD.GRP_F_TRAINING_PLAN.Chi_Phi_Dao_Tao} (trường tính trong Gold) / {GLD.GRP_F_TRAINING_PLAN.So_NS_Dao_Tao} (trường tính trong Gold)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(ThoiLuong, NgayKhaiGiang); dbo.TrainingStageEmployees(OrganizationId); dbo.Employees(Id); dbo.Glones(Code)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.Glones; nguồn ngoài SOL_HRM: BRZ.SOL.EXCEL.MAP_DEPT_TYPE
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_EMPLOYEES, SLV.SOL_MNL_DEPT_TYPE, SLV.SOL_HRM_GLONES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_F_EMPLOYEE_BALANCE, GLD.GRP_F_TRAINING_PLAN; tính trong Gold: Is_Month_Start, Is_Month_End, Snapshot_Date
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, D_COMPANY, F_TRAINING
6. **Thẻ v4:** KPI-044, KPI-084

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_PLAN

#### Kế hoạch đào tạo Hệ sinh thái

`PG-GRP-07-V20-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_CALENDAR, F_TRAINING_PROGRAM_PLAN
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số giờ đào tạo khối văn phòng và khối công trường

`PG-GRP-07-V11-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia.; Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân.

**Mã KPI**

KPI-040 Số giờ đào tạo; KPI-080 Giờ đào tạo bình quân / người

**Công thức tính**

```
KPI-040 = sum({GLD.SOL_F_TRAINING.Training_Hours}) | KPI-080 = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.SOL_D_TRAINING_COURSE.HC_Avg} (trường tính trong Gold))
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(EmployeeId, TinhTrang, TrainingStageId); dbo.TrainingStages(TenChuongTrinh)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_DEPARTMENT, F_EMPLOYEE_BALANCE, F_TRAINING
6. **Thẻ v4:** KPI-040, KPI-080

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Top 5 giảng viên nội bộ đứng lớp theo quý

`PG-GRP-07-V09-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5).

**Mã KPI**

KPI-111 Số lớp / giờ đứng lớp của giảng viên nội bộ

**Công thức tính**

```
KPI-111 = distinct_countIf({GLD.GRP_F_TRAINING_DELIVERY.Class_Key}, {GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor} = 1); sum({GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStages(NgayKhaiGiang)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStages
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_TRAINING_DELIVERY, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING_DELIVERY
6. **Thẻ v4:** KPI-111

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_DELIVERY.Instructor_Employee_Key, GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours, GLD.GRP_F_TRAINING_DELIVERY.Class_Key, GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_TRAINING_DELIVERY

#### Tần suất tổ chức các chương trình đào tạo trong năm

`PG-GRP-07-V04-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình).

**Mã KPI**

KPI-042 Số lớp đào tạo

**Công thức tính**

```
KPI-042 = distinct_count({GLD.SOL_F_TRAINING.Class_Key})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao, NgayKhaiGiang); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING
6. **Thẻ v4:** KPI-042

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số nhân sự tham gia đào tạo theo nhóm chương trình

`PG-GRP-07-V06-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp.; Lượt học viên: Số lượt tham gia có điểm danh.

**Mã KPI**

KPI-043 Số nhân sự tham gia đào tạo; KPI-083 Lượt học viên

**Công thức tính**

```
KPI-043 = distinct_countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1) | KPI-083 = countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.TrainingStageEmployees(EmployeeId, DiemDanh_TrangThai, TrainingStageId); dbo.TrainingStages(TenChuongTrinh, LoaiHinhDaoTao); dbo.OptionValues(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.TrainingStageEmployees, BRZ.SOL.HRM.TrainingStages, BRZ.SOL.HRM.OptionValues
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_TRAININGSTAGEEMPLOYEES, SLV.SOL_HRM_TRAININGSTAGES, SLV.SOL_HRM_OPTIONVALUES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_F_TRAINING, GLD.SOL_D_TRAINING_COURSE
5. **Mô hình chuẩn HST (CK2):** D_TRAINING_COURSE, F_TRAINING
6. **Thẻ v4:** KPI-043, KPI-083

**Lưu ý về mức sẵn sàng**

> Sẵn sàng: đủ cột nguồn trong SOL_HRM (qua LND → GLD)

#### Số nhân sự đang được kèm cặp theo chức danh

`PG-GRP-07-V12-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn.

**Mã KPI**

KPI-001 Tổng số nhân sự tại ngày chốt

**Công thức tính**

```
KPI-001 = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Month_End_Int} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** chưa khai báo
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_SUCCESSION_PLAN
6. **Thẻ v4:** KPI-001

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM

#### Nhân sự chưa có chứng chỉ tương ứng với chức danh

`PG-GRP-07-V03-HST` · Tổng HST · Đào tạo

**Cách tính nghiệp vụ**

Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh.

**Mã KPI**

KPI-085 Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh

**Công thức tính**

```
KPI-085 = distinct_countIf({GLD.GRP_F_CERTIFICATE.Employee_Key}, {GLD.GRP_F_CERTIFICATE.Du_CC_Bat_Buoc} (trường tính trong Gold) = 1) / distinct_count({GLD.GRP_F_CERTIFICATE.Employee_Key})
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** chưa có trong SOL_HRM
2. **BRZ (bản sao thô):** không có bảng SOL_HRM
3. **SLV (chuẩn hóa, đề xuất):** không có; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.GRP_F_CERTIFICATE
5. **Mô hình chuẩn HST (CK2):** D_JOBTITLE, F_CERTIFICATE
6. **Thẻ v4:** KPI-085

**Lưu ý về mức sẵn sàng**

> Nguồn: chưa có trong SOL_HRM (bảng đề xuất GLD.GRP_F_CERTIFICATE)

### Hồ sơ cán bộ quản lý · PG-GRP-10 (1 thẻ)

#### Hồ sơ năng lực cán bộ quản lý

`PG-GRP-10-V01-HST` · Tổng HST · Hồ sơ cán bộ quản lý

**Cách tính nghiệp vụ**

Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người

**Mã KPI**

Không gắn KPI (theo v4)

**Công thức tính**

```
Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_D_EMPLOYEE.Employee_Code}, {GLD.SOL_D_EMPLOYEE.Employee_Name}, {GLD.SOL_D_EMPLOYEE.DOB}, {GLD.SOL_D_EMPLOYEE.Seniority_Start_Date}, {GLD.SOL_D_JOBTITLE.JobTitle_Name}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key}
```

**Truy vết nguồn qua 6 tầng (raw SOL_HRM → … → KPI)**

1. **Raw SOL_HRM:** dbo.Employees(StaffCode, FullName, NgaySinh, NgayVaoCongTy, NgayVaoCongTyChinhThuc, ThoiGianKhongTinhThamNien); dbo.ContractLabors(TG_Tu); dbo.JobTitles(Name)
2. **BRZ (bản sao thô):** BRZ.SOL.HRM.Employees, BRZ.SOL.HRM.ContractLabors, BRZ.SOL.HRM.JobTitles
3. **SLV (chuẩn hóa, đề xuất):** SLV.SOL_HRM_EMPLOYEES, SLV.SOL_HRM_CONTRACTLABORS, SLV.SOL_HRM_JOBTITLES; SOL E&C hiện nạp LND → GLD
4. **GLD SOL:** GLD.SOL_D_EMPLOYEE, GLD.SOL_D_JOBTITLE, GLD.GRP_F_MANAGER_ASSIGNMENT
5. **Mô hình chuẩn HST (CK2):** D_COMPANY, D_JOBTITLE, F_MANAGER_ASSIGNMENT, F_SUCCESSION_PLAN
6. **Thẻ v4:** không gắn KPI

**Lưu ý về mức sẵn sàng**

> Sẵn sàng một phần: cột chưa có trong SOL_HRM: GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned, GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key; bảng đề xuất chưa có trong SOL_HRM: GLD.GRP_F_MANAGER_ASSIGNMENT

---