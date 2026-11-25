Định dạng mỗi dòng: `{tên KPI} = {Cách tính nghiệp vụ} = {Công thức tính}`. Thẻ có nhiều KPI được tách thành mỗi KPI một dòng.

_Nguồn: `congthuc_nguon_du_lieu.md` · 251 dòng công thức · 7 thẻ không gắn KPI (xem cuối file)_

## Cấp công ty · SOL E&C (132 dòng)

### Tổng quan · PG-GRP-01 · 15 dòng

```
Tổng nhân sự hiện tại = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Tổng nhân sự bình quân = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_D_CALENDAR.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Tuyển mới đang làm việc = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)

Năng suất lao động thực hiện / kế hoạch = NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân. = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.GRP_F_ANNUAL_PLAN.HC_Avg} (trường tính trong Gold))

Năng suất lao động thực hiện / kế hoạch = NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt. = sum({GLD.GRP_F_ANNUAL_PLAN.Productivity_Plan})

Năng suất lao động thực hiện / kế hoạch = % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch. = {GLD.GRP_F_ANNUAL_PLAN.NSLD_Dau_Nguoi} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.NSLD_KH} (trường tính trong Gold)

Nhân sự trung bình trong năm = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Cơ cấu nhân sự theo khối = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo khối = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo khối = Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị. = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [0, 4, 9]), "Văn phòng", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 2, 8]), "Công trường", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Nhà máy", "Khác")

Doanh thu và năng suất lao động theo tháng = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Năng suất lao động toàn công ty và theo khối = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 · 17 dòng

```
Kế hoạch doanh thu = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)

Doanh thu trực tiếp thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct})

Doanh thu trực tiếp thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu NSC thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct})

Doanh thu NSC thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))

Doanh thu theo từng lĩnh vực kinh doanh = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu theo từng lĩnh vực kinh doanh = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))

Doanh thu theo vùng miền = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_D_BUSINESS_LINE.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Top 10 dự án có năng suất lao động không đạt = NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị. = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.SOL_D_DEPARTMENT.HC_Avg_Du_An} (trường tính trong Gold))

Top 10 dự án có năng suất lao động không đạt = % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất). = {GLD.SOL_D_DEPARTMENT.NSLD_Du_An} (trường tính trong Gold) / maxIf({GLD.SOL_F_FINANCIAL_PLAN.Productivity_Plan}, {GLD.SOL_D_DEPARTMENT.Is_Latest_Int} (trường tính trong Gold) = 1)
```

### Quỹ lương & chi phí nhân sự · PG-GRP-09 · 18 dòng

```
Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000

Quỹ lương thực tế / quỹ lương kế hoạch = % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch. = periodToDateSum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {GLD.GRP_F_ANNUAL_PLAN.QL_KH} (trường tính trong Gold)

Quỹ lương kế hoạch / doanh thu kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000

Quỹ lương kế hoạch / doanh thu kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Quỹ lương thực tế / doanh thu thực tế = Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện. = (sum({GLD.SOL_F_SALARY.Total_Payroll_Fund}) + sum({GLD.SOL_F_SALARY.Company_Insurance})) / sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual})

Quỹ lương thực tế / doanh thu thực tế = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Quỹ lương thực tế / doanh thu thực tế = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Tổng quỹ lương theo tháng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Tổng quỹ lương theo tháng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Quỹ lương lũy kế so với kế hoạch năm = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Quỹ lương lũy kế so với kế hoạch năm = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa. = sum({GLD.SOL_F_SALARY.Total_Income})

Lương bình quân theo ngạch chức danh = Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code. = sum({GLD.SOL_F_SALARY.Actual_Salary}) / distinct_count(concat(toString({GLD.SOL_F_SALARY.Employee_Key}), toString({GLD.SOL_F_SALARY.Month_Key})))

Lương bình quân theo ngạch chức danh = Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ. = sum({GLD.SOL_F_SALARY.Total_Income}) / avg({GLD.SOL_F_EMPLOYEE_BALANCE.HC_Avg} (trường tính trong Gold))

Nhân sự có lương ngoài khung theo ngạch chức danh = Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc. = distinct_countIf({GLD.SOL_F_SALARY.Employee_Key}, {GLD.SOL_F_SALARY.Actual_Salary} < {GLD.GRP_D_SALARY_BAND.Band_Min} OR {GLD.SOL_F_SALARY.Actual_Salary} > {GLD.GRP_D_SALARY_BAND.Band_Max})
```

### Cơ cấu nhân sự · PG-GRP-03 · 14 dòng

```
Cơ cấu nhân sự theo cấp bậc = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo cấp bậc = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Cơ cấu nhân sự theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo chức danh = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Headcount theo phòng ban khối văn phòng = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Headcount theo phòng ban khối văn phòng = Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự. = sum({GLD.SOL_F_HEADCOUNT_PLAN.Headcount_Plan})

Headcount theo phòng ban khối văn phòng = Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu). = {GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold) - {GLD.SOL_D_DEPARTMENT.Dinh_Bien_KH} (trường tính trong Gold)

Cơ cấu nhân sự theo trường tốt nghiệp = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo trường tốt nghiệp = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo ngành nghề chuyên môn = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Cơ cấu nhân sự theo học vị = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo học vị = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

### Tuổi, thâm niên & vùng miền · PG-GRP-05 · 19 dòng

```
Cơ cấu nhân sự theo nhóm tuổi = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo nhóm tuổi = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo nhóm tuổi = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo thế hệ = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST. = ifelse({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 1, "< 1 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 3, "1 - 3 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 5, "3 - 5 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 7, "5 - 7 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 10, "7 - 10 năm", ">= 10 năm")

Cơ cấu nhân sự theo kinh nghiệm = Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Experience_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo kinh nghiệm = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nam – nữ = Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_D_EMPLOYEE.Gender} = "Nữ") / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)

Cơ cấu nam – nữ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))

Công tác trái miền (chỉ tính công trường) = Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Out_Of_Region} = 1)

Công tác trái miền (chỉ tính công trường) = Tỷ lệ trái vùng miền: K09 chia K01. = {GLD.SOL_D_EMPLOYEE.HC_Trai_Vung} (trường tính trong Gold) / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)

Nhân sự tuyển dụng theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Nhân sự tuyển dụng theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))

Nhân sự làm việc theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Nhân sự làm việc theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))
```

### Tuyển dụng · PG-GRP-06 · 15 dòng

```
Tuyển mới thực hiện / kế hoạch = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới thực hiện / kế hoạch = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {GLD.SOL_D_CALENDAR.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})

Tỷ lệ chuyển đổi nhận việc = Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc. = distinct_count({GLD.GRP_F_RECRUITMENT.Candidate_Key}) theo {GLD.GRP_F_RECRUITMENT.Stage}; avg(dateDiff({GLD.GRP_F_RECRUITMENT.Apply_Date}, {GLD.GRP_F_RECRUITMENT.Onboard_Date}, "DD"))

Tỷ lệ vượt qua thử việc = Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc. = countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Probation_Result} = "Đạt") / countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Is_Probation} = 1 AND isNotNull({GLD.GRP_F_CONTRACT.Probation_Result}))

Biến động ròng theo tháng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Biến động ròng theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Phễu tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Phễu tuyển dụng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Tuyển mới so với kế hoạch tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới so với kế hoạch tuyển dụng = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {GLD.GRP_F_RECRUITMENT_PLAN.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})

Cơ cấu nguồn tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Cơ cấu nguồn tuyển dụng = Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn. = sum({GLD.GRP_F_RECRUITMENT.Recruitment_Cost}) / sum({GLD.GRP_F_RECRUITMENT.Is_Hired})

Tuyển mới theo ngành nghề chuyên môn = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

### Nghỉ việc · PG-GRP-6B · 22 dòng

```
Tỷ lệ nghỉ việc lũy kế = Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ. = sum({GLD.SOL_D_CALENDAR.Terminate_Cnt} (trường tính trong Gold)) / avg({GLD.SOL_D_CALENDAR.HC_Avg} (trường tính trong Gold))

Tỷ lệ nghỉ việc lũy kế = Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt. = periodToDateSum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Effective_Date}, YEAR, ${pNgayChot})

Nghỉ việc tự nguyện so với không tự nguyện = Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0. = sumIf({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Voluntary} = "Active")

Nghỉ việc tự nguyện so với không tự nguyện = Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc. = {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Tu_Nguyen} (trường tính trong Gold) / {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Viec} (trường tính trong Gold)

Nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)

Nghỉ việc theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo tháng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nghỉ việc theo nhóm thâm niên = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo nhóm thâm niên = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nghỉ việc theo vùng miền quê quán = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo vùng miền quê quán = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới = Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm. = distinct_countIf({GLD.SOL_D_EMPLOYEE.Employee_Key}, {GLD.SOL_D_EMPLOYEE.Current_Status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {GLD.SOL_D_EMPLOYEE.DOB}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))

Nghỉ việc theo phòng ban – Khối Văn phòng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo phòng ban – Khối Văn phòng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nghỉ việc theo dự án – Khối Công trường = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo dự án – Khối Công trường = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nguyên nhân nghỉ việc = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nguyên nhân nghỉ việc = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Số lượng nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({GLD.GRP_F_RESIGNATION_REQUEST.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

### Đào tạo · PG-GRP-07 · 12 dòng

```
Số giờ đào tạo bình quân đầu người = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.GRP_F_TRAINING_PLAN.HC_Avg} (trường tính trong Gold))

Số giờ đào tạo bình quân đầu người = % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount). = sum({GLD.SOL_F_TRAINING.Training_Hours}) / sum({GLD.GRP_F_TRAINING_PLAN.Planned_Hours})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0). = sum({GLD.SOL_F_TRAINING.Training_Cost_Alloc})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo. = {GLD.GRP_F_TRAINING_PLAN.Chi_Phi_Dao_Tao} (trường tính trong Gold) / {GLD.GRP_F_TRAINING_PLAN.So_NS_Dao_Tao} (trường tính trong Gold)

Số giờ đào tạo khối văn phòng và khối công trường = Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia. = sum({GLD.SOL_F_TRAINING.Training_Hours})

Số giờ đào tạo khối văn phòng và khối công trường = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.SOL_D_TRAINING_COURSE.HC_Avg} (trường tính trong Gold))

Top 5 giảng viên nội bộ đứng lớp theo quý = Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5). = distinct_countIf({GLD.GRP_F_TRAINING_DELIVERY.Class_Key}, {GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor} = 1); sum({GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours})

Tần suất tổ chức các chương trình đào tạo trong năm = Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình). = distinct_count({GLD.SOL_F_TRAINING.Class_Key})

Số nhân sự tham gia đào tạo theo nhóm chương trình = Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp. = distinct_countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)

Số nhân sự tham gia đào tạo theo nhóm chương trình = Lượt học viên: Số lượt tham gia có điểm danh. = countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)

Số nhân sự đang được kèm cặp theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Nhân sự chưa có chứng chỉ tương ứng với chức danh = Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh. = distinct_countIf({GLD.GRP_F_CERTIFICATE.Employee_Key}, {GLD.GRP_F_CERTIFICATE.Du_CC_Bat_Buoc} (trường tính trong Gold) = 1) / distinct_count({GLD.GRP_F_CERTIFICATE.Employee_Key})
```

## Cấp hệ sinh thái · Tổng HST (119 dòng)

### Tổng quan · PG-GRP-01 · 13 dòng

```
Tổng nhân sự hiện tại = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Tổng nhân sự bình quân = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_CALENDAR.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_D_CALENDAR.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Tuyển mới đang làm việc = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)

Năng suất lao động thực hiện / kế hoạch = NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân. = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.GRP_F_ANNUAL_PLAN.HC_Avg} (trường tính trong Gold))

Năng suất lao động thực hiện / kế hoạch = NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt. = sum({GLD.GRP_F_ANNUAL_PLAN.Productivity_Plan})

Năng suất lao động thực hiện / kế hoạch = % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch. = {GLD.GRP_F_ANNUAL_PLAN.NSLD_Dau_Nguoi} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.NSLD_KH} (trường tính trong Gold)

Nhân sự trung bình trong năm = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Working_Days_In_Month} (trường tính trong Gold) >= 15 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Month_Date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Cơ cấu nhân sự theo khối = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo khối = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo khối = Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị. = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [0, 4, 9]), "Văn phòng", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 2, 8]), "Công trường", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Nhà máy", "Khác")
```

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 · 14 dòng

```
Kế hoạch doanh thu = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_TH_YTD} (trường tính trong Gold) / {GLD.GRP_F_ANNUAL_PLAN.Doanh_Thu_KH_Nam} (trường tính trong Gold)

Doanh thu trực tiếp thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct})

Doanh thu trực tiếp thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu NSC thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.SOL_F_PROJECT_REVENUE.Revenue_Type} = "Trực tiếp") / sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan_Direct})

Doanh thu NSC thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({GLD.SOL_D_DEPARTMENT.Org_Type}, [1, 8]), "Xây dựng", {GLD.SOL_D_DEPARTMENT.Org_Type} = 2, "M&E", in({GLD.SOL_D_DEPARTMENT.Org_Type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu theo từng lĩnh vực kinh doanh = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}))

Doanh thu theo vùng miền = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_D_BUSINESS_LINE.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Top 10 dự án có năng suất lao động không đạt = NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị. = (sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}) / 1000000000) / avg({GLD.SOL_D_DEPARTMENT.HC_Avg_Du_An} (trường tính trong Gold))

Top 10 dự án có năng suất lao động không đạt = % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất). = {GLD.SOL_D_DEPARTMENT.NSLD_Du_An} (trường tính trong Gold) / maxIf({GLD.SOL_F_FINANCIAL_PLAN.Productivity_Plan}, {GLD.SOL_D_DEPARTMENT.Is_Latest_Int} (trường tính trong Gold) = 1)
```

### Quỹ lương & chi phí nhân sự · PG-GRP-09 · 16 dòng

```
Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000

Quỹ lương thực tế / quỹ lương kế hoạch = % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch. = periodToDateSum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {GLD.GRP_F_ANNUAL_PLAN.QL_KH} (trường tính trong Gold)

Quỹ lương kế hoạch / doanh thu kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({GLD.GRP_F_ANNUAL_PLAN.Payroll_Plan}) / 1000000000

Quỹ lương kế hoạch / doanh thu kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({GLD.GRP_F_ANNUAL_PLAN.Revenue_Plan}) / 1000000000

Quỹ lương thực tế / doanh thu thực tế = Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện. = (sum({GLD.SOL_F_SALARY.Total_Payroll_Fund}) + sum({GLD.SOL_F_SALARY.Company_Insurance})) / sum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual})

Quỹ lương thực tế / doanh thu thực tế = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Quỹ lương thực tế / doanh thu thực tế = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({GLD.SOL_F_PROJECT_REVENUE.Revenue_Actual}, {GLD.GRP_F_ANNUAL_PLAN.Month_Date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Tổng quỹ lương theo tháng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Tổng quỹ lương theo tháng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({GLD.SOL_F_EMPLOYEE_EXPENSE.Component_Group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({GLD.SOL_F_EMPLOYEE_EXPENSE.Amount})

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa. = sum({GLD.SOL_F_SALARY.Total_Income})

Lương bình quân theo ngạch chức danh = Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code. = sum({GLD.SOL_F_SALARY.Actual_Salary}) / distinct_count(concat(toString({GLD.SOL_F_SALARY.Employee_Key}), toString({GLD.SOL_F_SALARY.Month_Key})))

Lương bình quân theo ngạch chức danh = Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ. = sum({GLD.SOL_F_SALARY.Total_Income}) / avg({GLD.SOL_F_EMPLOYEE_BALANCE.HC_Avg} (trường tính trong Gold))

Nhân sự có lương ngoài khung theo ngạch chức danh = Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc. = distinct_countIf({GLD.SOL_F_SALARY.Employee_Key}, {GLD.SOL_F_SALARY.Actual_Salary} < {GLD.GRP_D_SALARY_BAND.Band_Min} OR {GLD.SOL_F_SALARY.Actual_Salary} > {GLD.GRP_D_SALARY_BAND.Band_Max})
```

### Cơ cấu nhân sự · PG-GRP-03 · 14 dòng

```
Cơ cấu nhân sự theo cấp bậc = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo cấp bậc = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Cơ cấu nhân sự theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_LEVEL.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo chức danh = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chủ tịch") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Hội đồng chiến lược") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc khối"), "Giám đốc Khối", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám đốc dự án"), "Giám đốc dự án", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy trưởng"), "CHT", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chỉ huy phó"), "CHP", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng ban"), "Trưởng phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Trưởng bộ phận") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó phòng") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên gia"), "Chuyên gia", contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Giám sát chính") OR contains({GLD.SOL_D_JOBTITLE.JobTitle_Name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Headcount theo phòng ban khối văn phòng = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_DEPARTMENT.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Headcount theo phòng ban khối văn phòng = Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự. = sum({GLD.SOL_F_HEADCOUNT_PLAN.Headcount_Plan})

Headcount theo phòng ban khối văn phòng = Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu). = {GLD.SOL_D_DEPARTMENT.HC_Ngay_Chot} (trường tính trong Gold) - {GLD.SOL_D_DEPARTMENT.Dinh_Bien_KH} (trường tính trong Gold)

Cơ cấu nhân sự theo trường tốt nghiệp = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo trường tốt nghiệp = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo ngành nghề chuyên môn = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Cơ cấu nhân sự theo học vị = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo học vị = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

### Tuổi, thâm niên & vùng miền · PG-GRP-05 · 19 dòng

```
Cơ cấu nhân sự theo nhóm tuổi = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo nhóm tuổi = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo nhóm tuổi = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_AGING_GROUP.HC_Ngay_Chot} (trường tính trong Gold))

Cơ cấu nhân sự theo thế hệ = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Age}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_AGING_GROUP.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST. = ifelse({GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 1, "< 1 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 3, "1 - 3 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 5, "3 - 5 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 7, "5 - 7 năm", {GLD.SOL_F_EMPLOYEE_BALANCE.Tenure_Year} < 10, "7 - 10 năm", ">= 10 năm")

Cơ cấu nhân sự theo kinh nghiệm = Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount. = avgIf({GLD.SOL_F_EMPLOYEE_BALANCE.Experience_Year}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Cơ cấu nhân sự theo kinh nghiệm = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Phân bổ nhân sự theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Phân bổ nhân sự theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))

Số lượng nhân sự làm việc tại 3 miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_REGION.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Số lượng nhân sự làm việc tại 3 miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_REGION.HC_Ngay_Chot} (trường tính trong Gold))

Công tác trái miền (chỉ tính công trường) = Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Out_Of_Region} = 1)

Công tác trái miền (chỉ tính công trường) = Tỷ lệ trái vùng miền: K09 chia K01. = {GLD.SOL_D_EMPLOYEE.HC_Trai_Vung} (trường tính trong Gold) / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)

Cơ cấu nam – nữ = Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_D_EMPLOYEE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot} AND {GLD.SOL_D_EMPLOYEE.Gender} = "Nữ") / {GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold)

Cơ cấu nam – nữ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({GLD.SOL_D_EMPLOYEE.HC_Ngay_Chot} (trường tính trong Gold))
```

### Tuyển dụng · PG-GRP-06 · 13 dòng

```
Tuyển mới thực hiện / kế hoạch = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới thực hiện / kế hoạch = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {GLD.SOL_D_CALENDAR.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})

Tỷ lệ chuyển đổi nhận việc = Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc. = distinct_count({GLD.GRP_F_RECRUITMENT.Candidate_Key}) theo {GLD.GRP_F_RECRUITMENT.Stage}; avg(dateDiff({GLD.GRP_F_RECRUITMENT.Apply_Date}, {GLD.GRP_F_RECRUITMENT.Onboard_Date}, "DD"))

Tỷ lệ vượt qua thử việc = Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc. = countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Probation_Result} = "Đạt") / countIf({GLD.GRP_F_CONTRACT.Contract_ID}, {GLD.GRP_F_CONTRACT.Is_Probation} = 1 AND isNotNull({GLD.GRP_F_CONTRACT.Probation_Result}))

Biến động ròng theo tháng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Biến động ròng theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Tuyển mới so với kế hoạch tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới so với kế hoạch tuyển dụng = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {GLD.GRP_F_RECRUITMENT_PLAN.So_Tuyen_Moi} (trường tính trong Gold) / sum({GLD.GRP_F_RECRUITMENT_PLAN.Planned_Hires})

Cơ cấu nguồn tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Cơ cấu nguồn tuyển dụng = Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn. = sum({GLD.GRP_F_RECRUITMENT.Recruitment_Cost}) / sum({GLD.GRP_F_RECRUITMENT.Is_Hired})

Tuyển mới theo ngành nghề chuyên môn = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})

Tuyển mới theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({GLD.SOL_D_EMPLOYEE.Major}), "Nhân sự lĩnh vực khác", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu đường") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Giao thông") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({GLD.SOL_D_EMPLOYEE.Major}, "Điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Nhiệt") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Lạnh") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cấp thoát nước") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ điện") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "HVAC") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Tự động hóa"), "KS M&E", contains({GLD.SOL_D_EMPLOYEE.Major}, "Cơ khí") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Chế tạo máy"), "KS Cơ khí", contains({GLD.SOL_D_EMPLOYEE.Major}, "Kiến trúc"), "KTS", contains({GLD.SOL_D_EMPLOYEE.Major}, "Nội thất"), "Thiết kế nội thất", contains({GLD.SOL_D_EMPLOYEE.Major}, "Xây dựng") OR contains({GLD.SOL_D_EMPLOYEE.Major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_New_Hire})
```

### Nghỉ việc · PG-GRP-6B · 18 dòng

```
Tỷ lệ nghỉ việc lũy kế = Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ. = sum({GLD.SOL_D_CALENDAR.Terminate_Cnt} (trường tính trong Gold)) / avg({GLD.SOL_D_CALENDAR.HC_Avg} (trường tính trong Gold))

Tỷ lệ nghỉ việc lũy kế = Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt. = periodToDateSum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Effective_Date}, YEAR, ${pNgayChot})

Nghỉ việc tự nguyện so với không tự nguyện = Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0. = sumIf({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}, {GLD.SOL_F_EMPLOYEE_TRANSACTION.Voluntary} = "Active")

Nghỉ việc tự nguyện so với không tự nguyện = Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc. = {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Tu_Nguyen} (trường tính trong Gold) / {GLD.SOL_F_EMPLOYEE_TRANSACTION.So_Nghi_Viec} (trường tính trong Gold)

Nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)

Nghỉ việc theo nhóm thâm niên = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo nhóm thâm niên = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nghỉ việc theo vùng miền quê quán = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo vùng miền quê quán = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới = Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm. = distinct_countIf({GLD.SOL_D_EMPLOYEE.Employee_Key}, {GLD.SOL_D_EMPLOYEE.Current_Status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {GLD.SOL_D_EMPLOYEE.DOB}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))

Nguyên nhân nghỉ việc = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nguyên nhân nghỉ việc = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Nghỉ việc theo khối công trường và phòng ban văn phòng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Nghỉ việc theo khối công trường và phòng ban văn phòng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate}))

Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({GLD.SOL_F_EMPLOYEE_TRANSACTION.Is_Terminate})

Số lượng nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({GLD.GRP_F_RESIGNATION_REQUEST.Employee_Key}, dateDiff(${pNgayChot}, {GLD.GRP_F_RESIGNATION_REQUEST.Expected_Last_Date}, "MM") BETWEEN 0 AND 3)
```

### Đào tạo · PG-GRP-07 · 12 dòng

```
Số giờ đào tạo bình quân đầu người = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.GRP_F_TRAINING_PLAN.HC_Avg} (trường tính trong Gold))

Số giờ đào tạo bình quân đầu người = % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount). = sum({GLD.SOL_F_TRAINING.Training_Hours}) / sum({GLD.GRP_F_TRAINING_PLAN.Planned_Hours})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0). = sum({GLD.SOL_F_TRAINING.Training_Cost_Alloc})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo. = {GLD.GRP_F_TRAINING_PLAN.Chi_Phi_Dao_Tao} (trường tính trong Gold) / {GLD.GRP_F_TRAINING_PLAN.So_NS_Dao_Tao} (trường tính trong Gold)

Số giờ đào tạo khối văn phòng và khối công trường = Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia. = sum({GLD.SOL_F_TRAINING.Training_Hours})

Số giờ đào tạo khối văn phòng và khối công trường = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({GLD.SOL_F_TRAINING.Training_Hours}) / avg({GLD.SOL_D_TRAINING_COURSE.HC_Avg} (trường tính trong Gold))

Top 5 giảng viên nội bộ đứng lớp theo quý = Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5). = distinct_countIf({GLD.GRP_F_TRAINING_DELIVERY.Class_Key}, {GLD.GRP_F_TRAINING_DELIVERY.Is_Internal_Instructor} = 1); sum({GLD.GRP_F_TRAINING_DELIVERY.Teaching_Hours})

Tần suất tổ chức các chương trình đào tạo trong năm = Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình). = distinct_count({GLD.SOL_F_TRAINING.Class_Key})

Số nhân sự tham gia đào tạo theo nhóm chương trình = Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp. = distinct_countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)

Số nhân sự tham gia đào tạo theo nhóm chương trình = Lượt học viên: Số lượt tham gia có điểm danh. = countIf({GLD.SOL_F_TRAINING.Employee_Key}, {GLD.SOL_F_TRAINING.Is_Attended} = 1)

Số nhân sự đang được kèm cặp theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount} = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.IS_MONTH_END} (trường tính trong Gold) = 1 AND {GLD.SOL_F_EMPLOYEE_BALANCE.Snapshot_Date} = ${pNgayChot})

Nhân sự chưa có chứng chỉ tương ứng với chức danh = Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh. = distinct_countIf({GLD.GRP_F_CERTIFICATE.Employee_Key}, {GLD.GRP_F_CERTIFICATE.Du_CC_Bat_Buoc} (trường tính trong Gold) = 1) / distinct_count({GLD.GRP_F_CERTIFICATE.Employee_Key})
```

## Thẻ không gắn KPI (không có công thức KPI)

Các thẻ này đếm / nhóm trực tiếp trên cột dữ liệu, không có công thức KPI.

### Cấp công ty · SOL E&C

```
Kế hoạch đào tạo Hệ sinh thái = Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}

Giờ đào tạo theo phòng ban khối văn phòng = Giờ đào tạo lũy kế năm của từng phòng ban, kèm giờ / người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}

Giờ đào tạo theo dự án / công trường = Giờ đào tạo lũy kế năm của từng dự án, kèm giờ / người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}

Hồ sơ năng lực cán bộ quản lý = Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_D_EMPLOYEE.Employee_Code}, {GLD.SOL_D_EMPLOYEE.Employee_Name}, {GLD.SOL_D_EMPLOYEE.DOB}, {GLD.SOL_D_EMPLOYEE.Seniority_Start_Date}, {GLD.SOL_D_JOBTITLE.JobTitle_Name}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key}
```

### Cấp hệ sinh thái · Tổng HST

```
Sơ đồ tổ chức (số trên ô) = Số trên ô = đếm số nhân sự đang làm việc (headcount) của đơn vị và các đơn vị cấp dưới tại ngày chốt, mỗi người đếm 1 lần. = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_EMPLOYEE_BALANCE.Employee_Key}, {GLD.SOL_F_EMPLOYEE_BALANCE.Is_Headcount}, {GLD.SOL_F_EMPLOYEE_BALANCE.Dept_Key}, {GLD.SOL_D_DEPARTMENT.Dept_Key}, {GLD.SOL_D_DEPARTMENT.Parent_Dept_Key}

Kế hoạch đào tạo Hệ sinh thái = Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_F_TRAINING.Class_Key}, {GLD.SOL_D_TRAINING_COURSE.Class_Name}, {GLD.SOL_D_TRAINING_COURSE.Training_Type}, {GLD.SOL_D_TRAINING_COURSE.Start_Date}

Hồ sơ năng lực cán bộ quản lý = Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {GLD.SOL_D_EMPLOYEE.Employee_Code}, {GLD.SOL_D_EMPLOYEE.Employee_Name}, {GLD.SOL_D_EMPLOYEE.DOB}, {GLD.SOL_D_EMPLOYEE.Seniority_Start_Date}, {GLD.SOL_D_JOBTITLE.JobTitle_Name}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Revenue_Assigned}, {GLD.GRP_F_MANAGER_ASSIGNMENT.Project_Key}
```