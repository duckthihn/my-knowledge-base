Định dạng mỗi dòng: `{tên KPI} = {Cách tính nghiệp vụ} = {Công thức tính}`. Thẻ có nhiều KPI được tách thành mỗi KPI một dòng.

_Nguồn: `congthuc_nguon_du_lieu.md` (tên bảng đã đổi sang group_hr) · 251 dòng công thức · 7 thẻ không gắn KPI (xem cuối file)_

## Cấp công ty · SOL E&C (132 dòng)

### Tổng quan · PG-GRP-01 · 15 dòng

```
Tổng nhân sự hiện tại = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_calendar.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Tổng nhân sự bình quân = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_calendar.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.d_calendar.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Tuyển mới đang làm việc = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {group_hr.f_annual_plan.doanh_thu_th_ytd} (trường tính trong Gold) / {group_hr.f_annual_plan.doanh_thu_kh_nam} (trường tính trong Gold)

Năng suất lao động thực hiện / kế hoạch = NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân. = (sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000) / avg({group_hr.f_annual_plan.hc_avg} (trường tính trong Gold))

Năng suất lao động thực hiện / kế hoạch = NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt. = sum({group_hr.f_annual_plan.productivity_plan})

Năng suất lao động thực hiện / kế hoạch = % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch. = {group_hr.f_annual_plan.nsld_dau_nguoi} (trường tính trong Gold) / {group_hr.f_annual_plan.nsld_kh} (trường tính trong Gold)

Nhân sự trung bình trong năm = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.f_employee_balance.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Cơ cấu nhân sự theo khối = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_department.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo khối = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_department.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo khối = Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị. = ifelse(in({group_hr.d_department.org_type}, [0, 4, 9]), "Văn phòng", in({group_hr.d_department.org_type}, [1, 2, 8]), "Công trường", in({group_hr.d_department.org_type}, [3, 6]), "Nhà máy", "Khác")

Doanh thu và năng suất lao động theo tháng = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.f_employee_balance.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Năng suất lao động toàn công ty và theo khối = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.f_employee_balance.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))
```

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 · 17 dòng

```
Kế hoạch doanh thu = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {group_hr.f_annual_plan.doanh_thu_th_ytd} (trường tính trong Gold) / {group_hr.f_annual_plan.doanh_thu_kh_nam} (trường tính trong Gold)

Doanh thu trực tiếp thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_project_revenue.revenue_type} = "Trực tiếp") / sum({group_hr.f_annual_plan.revenue_plan_direct})

Doanh thu trực tiếp thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu NSC thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_project_revenue.revenue_type} = "Trực tiếp") / sum({group_hr.f_annual_plan.revenue_plan_direct})

Doanh thu NSC thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({group_hr.d_department.org_type}, [1, 8]), "Xây dựng", {group_hr.d_department.org_type} = 2, "M&E", in({group_hr.d_department.org_type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu thực hiện theo tháng – trực tiếp và nhà thầu phụ = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({group_hr.f_project_revenue.revenue_actual}))

Doanh thu theo từng lĩnh vực kinh doanh = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({group_hr.d_department.org_type}, [1, 8]), "Xây dựng", {group_hr.d_department.org_type} = 2, "M&E", in({group_hr.d_department.org_type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu theo từng lĩnh vực kinh doanh = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({group_hr.f_project_revenue.revenue_actual}))

Doanh thu theo vùng miền = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.d_business_line.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Top 10 dự án có năng suất lao động không đạt = NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị. = (sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000) / avg({group_hr.d_department.hc_avg_du_an} (trường tính trong Gold))

Top 10 dự án có năng suất lao động không đạt = % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất). = {group_hr.d_department.nsld_du_an} (trường tính trong Gold) / maxIf({group_hr.f_financial_plan.productivity_plan}, {group_hr.d_department.is_latest_int} (trường tính trong Gold) = 1)
```

### Quỹ lương & chi phí nhân sự · PG-GRP-09 · 18 dòng

```
Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({group_hr.f_annual_plan.payroll_plan}) / 1000000000

Quỹ lương thực tế / quỹ lương kế hoạch = % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch. = periodToDateSum({group_hr.f_employee_expense.amount}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {group_hr.f_annual_plan.ql_kh} (trường tính trong Gold)

Quỹ lương kế hoạch / doanh thu kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({group_hr.f_annual_plan.payroll_plan}) / 1000000000

Quỹ lương kế hoạch / doanh thu kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Quỹ lương thực tế / doanh thu thực tế = Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện. = (sum({group_hr.f_salary.total_payroll_fund}) + sum({group_hr.f_salary.company_insurance})) / sum({group_hr.f_project_revenue.revenue_actual})

Quỹ lương thực tế / doanh thu thực tế = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Quỹ lương thực tế / doanh thu thực tế = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Tổng quỹ lương theo tháng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Tổng quỹ lương theo tháng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({group_hr.f_employee_expense.component_group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({group_hr.f_employee_expense.component_group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Quỹ lương lũy kế so với kế hoạch năm = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Quỹ lương lũy kế so với kế hoạch năm = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({group_hr.f_employee_expense.component_group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({group_hr.f_employee_expense.component_group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({group_hr.f_employee_expense.component_group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({group_hr.f_employee_expense.component_group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa. = sum({group_hr.f_salary.total_income})

Lương bình quân theo ngạch chức danh = Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code. = sum({group_hr.f_salary.actual_salary}) / distinct_count(concat(toString({group_hr.f_salary.employee_key}), toString({group_hr.f_salary.month_key})))

Lương bình quân theo ngạch chức danh = Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ. = sum({group_hr.f_salary.total_income}) / avg({group_hr.f_employee_balance.hc_avg} (trường tính trong Gold))

Nhân sự có lương ngoài khung theo ngạch chức danh = Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc. = distinct_countIf({group_hr.f_salary.employee_key}, {group_hr.f_salary.actual_salary} < {group_hr.d_salary_band.band_min} OR {group_hr.f_salary.actual_salary} > {group_hr.d_salary_band.band_max})
```

### Cơ cấu nhân sự · PG-GRP-03 · 14 dòng

```
Cơ cấu nhân sự theo cấp bậc = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_level.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo cấp bậc = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({group_hr.d_jobtitle.jobtitle_name}, "Chủ tịch") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Hội đồng chiến lược") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc khối"), "Giám đốc Khối", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc dự án"), "Giám đốc dự án", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy trưởng"), "CHT", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy phó"), "CHP", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng ban"), "Trưởng phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng bộ phận") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia"), "Chuyên gia", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám sát chính") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Cơ cấu nhân sự theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_level.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo chức danh = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({group_hr.d_jobtitle.jobtitle_name}, "Chủ tịch") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Hội đồng chiến lược") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc khối"), "Giám đốc Khối", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc dự án"), "Giám đốc dự án", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy trưởng"), "CHT", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy phó"), "CHP", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng ban"), "Trưởng phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng bộ phận") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia"), "Chuyên gia", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám sát chính") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Headcount theo phòng ban khối văn phòng = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_department.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Headcount theo phòng ban khối văn phòng = Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự. = sum({group_hr.f_headcount_plan.headcount_plan})

Headcount theo phòng ban khối văn phòng = Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu). = {group_hr.d_department.hc_ngay_chot} (trường tính trong Gold) - {group_hr.d_department.dinh_bien_kh} (trường tính trong Gold)

Cơ cấu nhân sự theo trường tốt nghiệp = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo trường tốt nghiệp = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo ngành nghề chuyên môn = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({group_hr.d_employee.major}), "Nhân sự lĩnh vực khác", contains({group_hr.d_employee.major}, "Cầu đường") OR contains({group_hr.d_employee.major}, "Giao thông") OR contains({group_hr.d_employee.major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({group_hr.d_employee.major}, "Điện") OR contains({group_hr.d_employee.major}, "Nhiệt") OR contains({group_hr.d_employee.major}, "Lạnh") OR contains({group_hr.d_employee.major}, "Cấp thoát nước") OR contains({group_hr.d_employee.major}, "Cơ điện") OR contains({group_hr.d_employee.major}, "HVAC") OR contains({group_hr.d_employee.major}, "Tự động hóa"), "KS M&E", contains({group_hr.d_employee.major}, "Cơ khí") OR contains({group_hr.d_employee.major}, "Chế tạo máy"), "KS Cơ khí", contains({group_hr.d_employee.major}, "Kiến trúc"), "KTS", contains({group_hr.d_employee.major}, "Nội thất"), "Thiết kế nội thất", contains({group_hr.d_employee.major}, "Xây dựng") OR contains({group_hr.d_employee.major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Cơ cấu nhân sự theo học vị = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo học vị = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))
```

### Tuổi, thâm niên & vùng miền · PG-GRP-05 · 19 dòng

```
Cơ cấu nhân sự theo nhóm tuổi = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo nhóm tuổi = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_aging_group.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo nhóm tuổi = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.age}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_aging_group.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo thế hệ = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.age}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.tenure_year}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST. = ifelse({group_hr.f_employee_balance.tenure_year} < 1, "< 1 năm", {group_hr.f_employee_balance.tenure_year} < 3, "1 - 3 năm", {group_hr.f_employee_balance.tenure_year} < 5, "3 - 5 năm", {group_hr.f_employee_balance.tenure_year} < 7, "5 - 7 năm", {group_hr.f_employee_balance.tenure_year} < 10, "7 - 10 năm", ">= 10 năm")

Cơ cấu nhân sự theo kinh nghiệm = Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount. = avgIf({group_hr.f_employee_balance.experience_year}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo kinh nghiệm = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nam – nữ = Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot} AND {group_hr.d_employee.gender} = "Nữ") / {group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold)

Cơ cấu nam – nữ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))

Công tác trái miền (chỉ tính công trường) = Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot} AND {group_hr.f_employee_balance.is_out_of_region} = 1)

Công tác trái miền (chỉ tính công trường) = Tỷ lệ trái vùng miền: K09 chia K01. = {group_hr.d_employee.hc_trai_vung} (trường tính trong Gold) / {group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold)

Nhân sự tuyển dụng theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_region.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Nhân sự tuyển dụng theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_region.hc_ngay_chot} (trường tính trong Gold))

Nhân sự làm việc theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_region.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Nhân sự làm việc theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_region.hc_ngay_chot} (trường tính trong Gold))
```

### Tuyển dụng · PG-GRP-06 · 15 dòng

```
Tuyển mới thực hiện / kế hoạch = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới thực hiện / kế hoạch = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {group_hr.d_calendar.so_tuyen_moi} (trường tính trong Gold) / sum({group_hr.f_recruitment_plan.planned_hires})

Tỷ lệ chuyển đổi nhận việc = Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc. = distinct_count({group_hr.f_recruitment.candidate_key}) theo {group_hr.f_recruitment.stage}; avg(dateDiff({group_hr.f_recruitment.apply_date}, {group_hr.f_recruitment.onboard_date}, "DD"))

Tỷ lệ vượt qua thử việc = Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc. = countIf({group_hr.f_contract.contract_id}, {group_hr.f_contract.probation_result} = "Đạt") / countIf({group_hr.f_contract.contract_id}, {group_hr.f_contract.is_probation} = 1 AND isNotNull({group_hr.f_contract.probation_result}))

Biến động ròng theo tháng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Biến động ròng theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Phễu tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Phễu tuyển dụng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Tuyển mới so với kế hoạch tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới so với kế hoạch tuyển dụng = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {group_hr.f_recruitment_plan.so_tuyen_moi} (trường tính trong Gold) / sum({group_hr.f_recruitment_plan.planned_hires})

Cơ cấu nguồn tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Cơ cấu nguồn tuyển dụng = Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn. = sum({group_hr.f_recruitment.recruitment_cost}) / sum({group_hr.f_recruitment.is_hired})

Tuyển mới theo ngành nghề chuyên môn = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({group_hr.d_employee.major}), "Nhân sự lĩnh vực khác", contains({group_hr.d_employee.major}, "Cầu đường") OR contains({group_hr.d_employee.major}, "Giao thông") OR contains({group_hr.d_employee.major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({group_hr.d_employee.major}, "Điện") OR contains({group_hr.d_employee.major}, "Nhiệt") OR contains({group_hr.d_employee.major}, "Lạnh") OR contains({group_hr.d_employee.major}, "Cấp thoát nước") OR contains({group_hr.d_employee.major}, "Cơ điện") OR contains({group_hr.d_employee.major}, "HVAC") OR contains({group_hr.d_employee.major}, "Tự động hóa"), "KS M&E", contains({group_hr.d_employee.major}, "Cơ khí") OR contains({group_hr.d_employee.major}, "Chế tạo máy"), "KS Cơ khí", contains({group_hr.d_employee.major}, "Kiến trúc"), "KTS", contains({group_hr.d_employee.major}, "Nội thất"), "Thiết kế nội thất", contains({group_hr.d_employee.major}, "Xây dựng") OR contains({group_hr.d_employee.major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})
```

### Nghỉ việc · PG-GRP-6B · 22 dòng

```
Tỷ lệ nghỉ việc lũy kế = Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ. = sum({group_hr.d_calendar.terminate_cnt} (trường tính trong Gold)) / avg({group_hr.d_calendar.hc_avg} (trường tính trong Gold))

Tỷ lệ nghỉ việc lũy kế = Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt. = periodToDateSum({group_hr.f_employee_transaction.is_terminate}, {group_hr.f_employee_transaction.effective_date}, YEAR, ${pNgayChot})

Nghỉ việc tự nguyện so với không tự nguyện = Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0. = sumIf({group_hr.f_employee_transaction.is_terminate}, {group_hr.f_employee_transaction.voluntary} = "Active")

Nghỉ việc tự nguyện so với không tự nguyện = Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc. = {group_hr.f_employee_transaction.so_nghi_tu_nguyen} (trường tính trong Gold) / {group_hr.f_employee_transaction.so_nghi_viec} (trường tính trong Gold)

Nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, dateDiff(${pNgayChot}, {group_hr.f_resignation_request.expected_last_date}, "MM") BETWEEN 0 AND 3)

Nghỉ việc theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo tháng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nghỉ việc theo nhóm thâm niên = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo nhóm thâm niên = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nghỉ việc theo vùng miền quê quán = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo vùng miền quê quán = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới = Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm. = distinct_countIf({group_hr.d_employee.employee_key}, {group_hr.d_employee.current_status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {group_hr.d_employee.dob}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))

Nghỉ việc theo phòng ban – Khối Văn phòng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo phòng ban – Khối Văn phòng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nghỉ việc theo dự án – Khối Công trường = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo dự án – Khối Công trường = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nguyên nhân nghỉ việc = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nguyên nhân nghỉ việc = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Số lượng nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({group_hr.f_resignation_request.employee_key}, dateDiff(${pNgayChot}, {group_hr.f_resignation_request.expected_last_date}, "MM") BETWEEN 0 AND 3)
```

### Đào tạo · PG-GRP-07 · 12 dòng

```
Số giờ đào tạo bình quân đầu người = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({group_hr.f_training.training_hours}) / avg({group_hr.f_training_plan.hc_avg} (trường tính trong Gold))

Số giờ đào tạo bình quân đầu người = % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount). = sum({group_hr.f_training.training_hours}) / sum({group_hr.f_training_plan.planned_hours})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0). = sum({group_hr.f_training.training_cost_alloc})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo. = {group_hr.f_training_plan.chi_phi_dao_tao} (trường tính trong Gold) / {group_hr.f_training_plan.so_ns_dao_tao} (trường tính trong Gold)

Số giờ đào tạo khối văn phòng và khối công trường = Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia. = sum({group_hr.f_training.training_hours})

Số giờ đào tạo khối văn phòng và khối công trường = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({group_hr.f_training.training_hours}) / avg({group_hr.d_training_course.hc_avg} (trường tính trong Gold))

Top 5 giảng viên nội bộ đứng lớp theo quý = Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5). = distinct_countIf({group_hr.f_training_delivery.class_key}, {group_hr.f_training_delivery.is_internal_instructor} = 1); sum({group_hr.f_training_delivery.teaching_hours})

Tần suất tổ chức các chương trình đào tạo trong năm = Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình). = distinct_count({group_hr.f_training.class_key})

Số nhân sự tham gia đào tạo theo nhóm chương trình = Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp. = distinct_countIf({group_hr.f_training.employee_key}, {group_hr.f_training.is_attended} = 1)

Số nhân sự tham gia đào tạo theo nhóm chương trình = Lượt học viên: Số lượt tham gia có điểm danh. = countIf({group_hr.f_training.employee_key}, {group_hr.f_training.is_attended} = 1)

Số nhân sự đang được kèm cặp theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Nhân sự chưa có chứng chỉ tương ứng với chức danh = Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh. = distinct_countIf({group_hr.f_certificate.employee_key}, {group_hr.f_certificate.du_cc_bat_buoc} (trường tính trong Gold) = 1) / distinct_count({group_hr.f_certificate.employee_key})
```

## Cấp hệ sinh thái · Tổng HST (119 dòng)

### Tổng quan · PG-GRP-01 · 13 dòng

```
Tổng nhân sự hiện tại = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_calendar.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Tổng nhân sự bình quân = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_calendar.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.d_calendar.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Tuyển mới đang làm việc = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {group_hr.f_annual_plan.doanh_thu_th_ytd} (trường tính trong Gold) / {group_hr.f_annual_plan.doanh_thu_kh_nam} (trường tính trong Gold)

Năng suất lao động thực hiện / kế hoạch = NSLĐ / đầu người (tỷ đồng/người): Doanh thu thực hiện (tỷ đồng) chia nhân sự bình quân. = (sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000) / avg({group_hr.f_annual_plan.hc_avg} (trường tính trong Gold))

Năng suất lao động thực hiện / kế hoạch = NSLĐ kế hoạch năm (tỷ đồng/người): Chỉ tiêu NSLĐ kế hoạch năm do công ty đặt. = sum({group_hr.f_annual_plan.productivity_plan})

Năng suất lao động thực hiện / kế hoạch = % đạt NSLĐ kế hoạch: NSLĐ thực hiện / NSLĐ kế hoạch. = {group_hr.f_annual_plan.nsld_dau_nguoi} (trường tính trong Gold) / {group_hr.f_annual_plan.nsld_kh} (trường tính trong Gold)

Nhân sự trung bình trong năm = Tổng nhân sự bình quân: Nhân sự bình quân tháng = số nhân sự làm việc từ 15 ngày trở lên trong tháng; người vào làm hoặc nghỉ việc trong tháng chỉ được tính khi đủ 15 ngày. Thẻ KPI lấy bình quân các tháng của năm đang chọn (đến tháng chốt); biểu đồ hiển thị từng tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.working_days_in_month} (trường tính trong Gold) >= 15 AND {group_hr.f_employee_balance.month_date} (trường tính trong Gold) = truncDate("MM", ${pNgayChot}))

Cơ cấu nhân sự theo khối = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_department.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo khối = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_department.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo khối = Khối HST (VP / Công trường / Nhà máy): Xếp đơn vị vào Văn phòng / Công trường / Nhà máy theo mã loại đơn vị. = ifelse(in({group_hr.d_department.org_type}, [0, 4, 9]), "Văn phòng", in({group_hr.d_department.org_type}, [1, 2, 8]), "Công trường", in({group_hr.d_department.org_type}, [3, 6]), "Nhà máy", "Khác")
```

### Doanh thu theo lĩnh vực & dự án · PG-GRP-02 · 14 dòng

```
Kế hoạch doanh thu = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu thực hiện / kế hoạch năm = % hoàn thành kế hoạch doanh thu lũy kế năm: Doanh thu thực hiện lũy kế năm / doanh thu kế hoạch năm. = {group_hr.f_annual_plan.doanh_thu_th_ytd} (trường tính trong Gold) / {group_hr.f_annual_plan.doanh_thu_kh_nam} (trường tính trong Gold)

Doanh thu trực tiếp thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_project_revenue.revenue_type} = "Trực tiếp") / sum({group_hr.f_annual_plan.revenue_plan_direct})

Doanh thu trực tiếp thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu NSC thực hiện / kế hoạch = Doanh thu trực tiếp / NSC so kế hoạch: Doanh thu thực hiện từng loại chia kế hoạch tương ứng (chưa dựng được). = sumIf({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_project_revenue.revenue_type} = "Trực tiếp") / sum({group_hr.f_annual_plan.revenue_plan_direct})

Doanh thu NSC thực hiện / kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Doanh thu thực hiện (tỷ đồng): Tổng doanh thu thực tế CCM trong kỳ, tỷ đồng. = sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000

Doanh thu theo từng lĩnh vực kinh doanh = Lĩnh vực kinh doanh (theo loại đơn vị): Xếp doanh thu vào lĩnh vực theo loại đơn vị. = ifelse(in({group_hr.d_department.org_type}, [1, 8]), "Xây dựng", {group_hr.d_department.org_type} = 2, "M&E", in({group_hr.d_department.org_type}, [3, 6]), "Leafseal", "Chưa xác định")

Doanh thu theo từng lĩnh vực kinh doanh = % doanh thu theo lĩnh vực: Tỷ trọng doanh thu từng lĩnh vực. = percentOfTotal(sum({group_hr.f_project_revenue.revenue_actual}))

Doanh thu theo vùng miền = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.d_business_line.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Top 10 dự án có năng suất lao động không đạt = NSLĐ theo đơn vị / dự án: Doanh thu thực hiện (tỷ) / headcount bình quân của dự án/đơn vị. = (sum({group_hr.f_project_revenue.revenue_actual}) / 1000000000) / avg({group_hr.d_department.hc_avg_du_an} (trường tính trong Gold))

Top 10 dự án có năng suất lao động không đạt = % đạt NSLĐ kế hoạch của dự án: NSLĐ dự án / NSLĐ kế hoạch dự án (định mức BCTC mới nhất). = {group_hr.d_department.nsld_du_an} (trường tính trong Gold) / maxIf({group_hr.f_financial_plan.productivity_plan}, {group_hr.d_department.is_latest_int} (trường tính trong Gold) = 1)
```

### Quỹ lương & chi phí nhân sự · PG-GRP-09 · 16 dòng

```
Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Quỹ lương thực tế / quỹ lương kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({group_hr.f_annual_plan.payroll_plan}) / 1000000000

Quỹ lương thực tế / quỹ lương kế hoạch = % quỹ lương thực hiện so kế hoạch/ngân sách: Quỹ lương thực hiện lũy kế / quỹ lương kế hoạch. = periodToDateSum({group_hr.f_employee_expense.amount}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000 / {group_hr.f_annual_plan.ql_kh} (trường tính trong Gold)

Quỹ lương kế hoạch / doanh thu kế hoạch = Quỹ lương kế hoạch năm: Quỹ lương kế hoạch được duyệt của năm. = sum({group_hr.f_annual_plan.payroll_plan}) / 1000000000

Quỹ lương kế hoạch / doanh thu kế hoạch = Tổng doanh thu kế hoạch năm: Tổng doanh thu kế hoạch, tỷ đồng (chưa có nguồn). = sum({group_hr.f_annual_plan.revenue_plan}) / 1000000000

Quỹ lương thực tế / doanh thu thực tế = Tỷ lệ quỹ lương / doanh thu: (Quỹ lương + BH công ty đóng) chia doanh thu thực hiện. = (sum({group_hr.f_salary.total_payroll_fund}) + sum({group_hr.f_salary.company_insurance})) / sum({group_hr.f_project_revenue.revenue_actual})

Quỹ lương thực tế / doanh thu thực tế = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Quỹ lương thực tế / doanh thu thực tế = Doanh thu thực hiện lũy kế năm: Doanh thu thực hiện từ đầu năm đến ngày chốt, tỷ đồng. = periodToDateSum({group_hr.f_project_revenue.revenue_actual}, {group_hr.f_annual_plan.month_date} (trường tính trong Gold), YEAR, ${pNgayChot}) / 1000000000

Tổng quỹ lương theo tháng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Tổng quỹ lương theo tháng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({group_hr.f_employee_expense.component_group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({group_hr.f_employee_expense.component_group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Nhóm quỹ lương HST (Lương / Thưởng / Phúc lợi): Xếp khoản mục lương vào Lương / Thưởng / Phúc lợi. = ifelse(in({group_hr.f_employee_expense.component_group}, ["Lương", "Lương làm việc ngày lễ"]), "Lương", in({group_hr.f_employee_expense.component_group}, ["Thưởng", "Thưởng (non-cash)"]), "Thưởng", "Phúc lợi theo lương")

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Quỹ lương đã thực hiện: Tổng các khoản mục chi phí nhân sự (tạm từ bảng lương). = sum({group_hr.f_employee_expense.amount})

Cơ cấu thu nhập: lương – phụ cấp – thưởng = Tổng thu nhập: Tổng thu nhập trong tháng của nhân viên trên bảng lương đã khóa. = sum({group_hr.f_salary.total_income})

Lương bình quân theo ngạch chức danh = Lương bình quân theo cấp bậc: Lương thực tế bình quân / người / tháng theo Level_Code. = sum({group_hr.f_salary.actual_salary}) / distinct_count(concat(toString({group_hr.f_salary.employee_key}), toString({group_hr.f_salary.month_key})))

Lương bình quân theo ngạch chức danh = Lương bình quân / người: Tổng thu nhập chia nhân sự bình quân của cùng kỳ. = sum({group_hr.f_salary.total_income}) / avg({group_hr.f_employee_balance.hc_avg} (trường tính trong Gold))

Nhân sự có lương ngoài khung theo ngạch chức danh = Số NS có lương ngoài khung: Số nhân viên có lương < Band_Min hoặc > Band_Max của cấp bậc. = distinct_countIf({group_hr.f_salary.employee_key}, {group_hr.f_salary.actual_salary} < {group_hr.d_salary_band.band_min} OR {group_hr.f_salary.actual_salary} > {group_hr.d_salary_band.band_max})
```

### Cơ cấu nhân sự · PG-GRP-03 · 14 dòng

```
Cơ cấu nhân sự theo cấp bậc = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_level.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo cấp bậc = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({group_hr.d_jobtitle.jobtitle_name}, "Chủ tịch") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Hội đồng chiến lược") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc khối"), "Giám đốc Khối", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc dự án"), "Giám đốc dự án", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy trưởng"), "CHT", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy phó"), "CHP", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng ban"), "Trưởng phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng bộ phận") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia"), "Chuyên gia", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám sát chính") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Cơ cấu nhân sự theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_level.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo chức danh = Nhóm vị trí HST (theo tên chức danh): Xếp nhân viên vào nhóm vị trí theo tên chức danh. = ifelse(contains({group_hr.d_jobtitle.jobtitle_name}, "Chủ tịch") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Hội đồng chiến lược") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Tổng giám đốc"), "Ban lãnh đạo", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc điều hành"), "Giám đốc điều hành", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc khối"), "Giám đốc Khối", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám đốc dự án"), "Giám đốc dự án", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy trưởng"), "CHT", contains({group_hr.d_jobtitle.jobtitle_name}, "Chỉ huy phó"), "CHP", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng ban"), "Trưởng phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Trưởng bộ phận") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó phòng") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Phó ban"), "Trưởng bộ phận/ Phó phòng ban", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia cao cấp"), "Chuyên gia cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên cao cấp"), "Chuyên viên cao cấp", contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên gia"), "Chuyên gia", contains({group_hr.d_jobtitle.jobtitle_name}, "Giám sát chính") OR contains({group_hr.d_jobtitle.jobtitle_name}, "Chuyên viên"), "Giám sát chính/ Chuyên viên", "Giám sát/ nhân viên")

Headcount theo phòng ban khối văn phòng = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_department.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Headcount theo phòng ban khối văn phòng = Định biên kế hoạch: Tổng số nhân sự kế hoạch theo kế hoạch nhân sự. = sum({group_hr.f_headcount_plan.headcount_plan})

Headcount theo phòng ban khối văn phòng = Thừa / thiếu so định biên: Headcount thực tế − định biên kế hoạch (âm = thiếu). = {group_hr.d_department.hc_ngay_chot} (trường tính trong Gold) - {group_hr.d_department.dinh_bien_kh} (trường tính trong Gold)

Cơ cấu nhân sự theo trường tốt nghiệp = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo trường tốt nghiệp = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo ngành nghề chuyên môn = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({group_hr.d_employee.major}), "Nhân sự lĩnh vực khác", contains({group_hr.d_employee.major}, "Cầu đường") OR contains({group_hr.d_employee.major}, "Giao thông") OR contains({group_hr.d_employee.major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({group_hr.d_employee.major}, "Điện") OR contains({group_hr.d_employee.major}, "Nhiệt") OR contains({group_hr.d_employee.major}, "Lạnh") OR contains({group_hr.d_employee.major}, "Cấp thoát nước") OR contains({group_hr.d_employee.major}, "Cơ điện") OR contains({group_hr.d_employee.major}, "HVAC") OR contains({group_hr.d_employee.major}, "Tự động hóa"), "KS M&E", contains({group_hr.d_employee.major}, "Cơ khí") OR contains({group_hr.d_employee.major}, "Chế tạo máy"), "KS Cơ khí", contains({group_hr.d_employee.major}, "Kiến trúc"), "KTS", contains({group_hr.d_employee.major}, "Nội thất"), "Thiết kế nội thất", contains({group_hr.d_employee.major}, "Xây dựng") OR contains({group_hr.d_employee.major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Cơ cấu nhân sự theo học vị = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo học vị = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))
```

### Tuổi, thâm niên & vùng miền · PG-GRP-05 · 19 dòng

```
Cơ cấu nhân sự theo nhóm tuổi = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo nhóm tuổi = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_aging_group.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo nhóm tuổi = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.age}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thế hệ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_aging_group.hc_ngay_chot} (trường tính trong Gold))

Cơ cấu nhân sự theo thế hệ = Tuổi bình quân: Tuổi trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.age}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_aging_group.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Thâm niên bình quân (năm): Số năm thâm niên trung bình của nhân sự headcount tại ngày chốt. = avgIf({group_hr.f_employee_balance.tenure_year}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo thâm niên = Nhóm thâm niên HST: Xếp nhân viên vào nhóm thâm niên theo ngưỡng HST. = ifelse({group_hr.f_employee_balance.tenure_year} < 1, "< 1 năm", {group_hr.f_employee_balance.tenure_year} < 3, "1 - 3 năm", {group_hr.f_employee_balance.tenure_year} < 5, "3 - 5 năm", {group_hr.f_employee_balance.tenure_year} < 7, "5 - 7 năm", {group_hr.f_employee_balance.tenure_year} < 10, "7 - 10 năm", ">= 10 năm")

Cơ cấu nhân sự theo kinh nghiệm = Số năm kinh nghiệm bình quân: Bình quân Experience_Year của headcount. = avgIf({group_hr.f_employee_balance.experience_year}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Cơ cấu nhân sự theo kinh nghiệm = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Phân bổ nhân sự theo vùng miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_region.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Phân bổ nhân sự theo vùng miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_region.hc_ngay_chot} (trường tính trong Gold))

Số lượng nhân sự làm việc tại 3 miền = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_region.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Số lượng nhân sự làm việc tại 3 miền = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_region.hc_ngay_chot} (trường tính trong Gold))

Công tác trái miền (chỉ tính công trường) = Số NS trái vùng miền: Số người làm việc ở vùng khác vùng điểm gốc. Hiện luôn = 0. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot} AND {group_hr.f_employee_balance.is_out_of_region} = 1)

Công tác trái miền (chỉ tính công trường) = Tỷ lệ trái vùng miền: K09 chia K01. = {group_hr.d_employee.hc_trai_vung} (trường tính trong Gold) / {group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold)

Cơ cấu nam – nữ = Tỷ lệ nữ: Số nhân sự nữ / tổng headcount tại ngày chốt. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.d_employee.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot} AND {group_hr.d_employee.gender} = "Nữ") / {group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold)

Cơ cấu nam – nữ = % cơ cấu theo chiều đang chọn: Tỷ trọng số nhân sự của từng nhóm (ngành, khối, vị trí, tuổi…) trên tổng tại ngày chốt. = percentOfTotal({group_hr.d_employee.hc_ngay_chot} (trường tính trong Gold))
```

### Tuyển dụng · PG-GRP-06 · 13 dòng

```
Tuyển mới thực hiện / kế hoạch = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới thực hiện / kế hoạch = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {group_hr.d_calendar.so_tuyen_moi} (trường tính trong Gold) / sum({group_hr.f_recruitment_plan.planned_hires})

Tỷ lệ chuyển đổi nhận việc = Tỷ lệ chuyển đổi phễu & thời gian tuyển TB: Ứng viên theo vòng, tỷ lệ chuyển đổi giữa vòng, số ngày TB từ ứng tuyển đến nhận việc. = distinct_count({group_hr.f_recruitment.candidate_key}) theo {group_hr.f_recruitment.stage}; avg(dateDiff({group_hr.f_recruitment.apply_date}, {group_hr.f_recruitment.onboard_date}, "DD"))

Tỷ lệ vượt qua thử việc = Tỷ lệ vượt qua thử việc: Số NS đạt thử việc / số NS kết thúc thử việc. = countIf({group_hr.f_contract.contract_id}, {group_hr.f_contract.probation_result} = "Đạt") / countIf({group_hr.f_contract.contract_id}, {group_hr.f_contract.is_probation} = 1 AND isNotNull({group_hr.f_contract.probation_result}))

Biến động ròng theo tháng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Biến động ròng theo tháng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Tuyển mới so với kế hoạch tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới so với kế hoạch tuyển dụng = % hoàn thành kế hoạch tuyển: Tuyển mới / kế hoạch tuyển theo tháng, đơn vị. = {group_hr.f_recruitment_plan.so_tuyen_moi} (trường tính trong Gold) / sum({group_hr.f_recruitment_plan.planned_hires})

Cơ cấu nguồn tuyển dụng = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Cơ cấu nguồn tuyển dụng = Chi phí tuyển / người theo nguồn: Chi phí tuyển / số người nhận việc theo nguồn. = sum({group_hr.f_recruitment.recruitment_cost}) / sum({group_hr.f_recruitment.is_hired})

Tuyển mới theo ngành nghề chuyên môn = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})

Tuyển mới theo ngành nghề chuyên môn = Nhóm ngành HST (theo chuyên ngành): Xếp nhân viên vào nhóm ngành theo từ khóa chuyên ngành. = ifelse(isNull({group_hr.d_employee.major}), "Nhân sự lĩnh vực khác", contains({group_hr.d_employee.major}, "Cầu đường") OR contains({group_hr.d_employee.major}, "Giao thông") OR contains({group_hr.d_employee.major}, "Cầu hầm"), "Kỹ sư cầu đường", contains({group_hr.d_employee.major}, "Điện") OR contains({group_hr.d_employee.major}, "Nhiệt") OR contains({group_hr.d_employee.major}, "Lạnh") OR contains({group_hr.d_employee.major}, "Cấp thoát nước") OR contains({group_hr.d_employee.major}, "Cơ điện") OR contains({group_hr.d_employee.major}, "HVAC") OR contains({group_hr.d_employee.major}, "Tự động hóa"), "KS M&E", contains({group_hr.d_employee.major}, "Cơ khí") OR contains({group_hr.d_employee.major}, "Chế tạo máy"), "KS Cơ khí", contains({group_hr.d_employee.major}, "Kiến trúc"), "KTS", contains({group_hr.d_employee.major}, "Nội thất"), "Thiết kế nội thất", contains({group_hr.d_employee.major}, "Xây dựng") OR contains({group_hr.d_employee.major}, "Công trình"), "KS Xây dựng", "Nhân sự lĩnh vực khác")

Top 5 tỉnh thành có nhân sự tuyển dụng nhiều nhất = Số tuyển mới: Số lượt tuyển mới trong kỳ. = sum({group_hr.f_employee_transaction.is_new_hire})
```

### Nghỉ việc · PG-GRP-6B · 18 dòng

```
Tỷ lệ nghỉ việc lũy kế = Tỷ lệ nghỉ việc (turnover): Số lượt nghỉ việc chia nhân sự bình quân của cùng kỳ. = sum({group_hr.d_calendar.terminate_cnt} (trường tính trong Gold)) / avg({group_hr.d_calendar.hc_avg} (trường tính trong Gold))

Tỷ lệ nghỉ việc lũy kế = Số nghỉ việc lũy kế năm: Số lượt nghỉ việc từ đầu năm đến ngày chốt. = periodToDateSum({group_hr.f_employee_transaction.is_terminate}, {group_hr.f_employee_transaction.effective_date}, YEAR, ${pNgayChot})

Nghỉ việc tự nguyện so với không tự nguyện = Số nghỉ việc tự nguyện: Số lượt nghỉ việc được xếp tự nguyện. Hiện luôn = 0. = sumIf({group_hr.f_employee_transaction.is_terminate}, {group_hr.f_employee_transaction.voluntary} = "Active")

Nghỉ việc tự nguyện so với không tự nguyện = Tỷ lệ nghỉ việc tự nguyện: Số nghỉ tự nguyện / tổng nghỉ việc. = {group_hr.f_employee_transaction.so_nghi_tu_nguyen} (trường tính trong Gold) / {group_hr.f_employee_transaction.so_nghi_viec} (trường tính trong Gold)

Nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({group_hr.f_employee_balance.employee_key}, dateDiff(${pNgayChot}, {group_hr.f_resignation_request.expected_last_date}, "MM") BETWEEN 0 AND 3)

Nghỉ việc theo nhóm thâm niên = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo nhóm thâm niên = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nghỉ việc theo vùng miền quê quán = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo vùng miền quê quán = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nhân sự đến tuổi nghỉ hưu trong 1 – 3 – 5 năm tới = Số NS đến tuổi nghỉ hưu trong 1–3–5 năm: Số NS đang làm việc sẽ đạt tuổi nghỉ hưu (theo giới, lộ trình NĐ 135/2020) trong N năm. = distinct_countIf({group_hr.d_employee.employee_key}, {group_hr.d_employee.current_status} = "Đang làm việc" AND addDateTime(${pTuoiHuuThang}, "MM", {group_hr.d_employee.dob}) <= addDateTime(${pSoNam}, "YYYY", ${pNgayChot}))

Nguyên nhân nghỉ việc = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nguyên nhân nghỉ việc = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Nghỉ việc theo khối công trường và phòng ban văn phòng = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Nghỉ việc theo khối công trường và phòng ban văn phòng = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Top 5 trường đại học có nhân sự nghỉ việc nhiều nhất = % nghỉ việc theo nhóm (Top 5): Tỷ trọng lượt nghỉ của từng nhóm (trường, lý do, vùng) trên tổng lượt nghỉ. = percentOfTotal(sum({group_hr.f_employee_transaction.is_terminate}))

Top 5 tỉnh thành có nhân sự nghỉ việc nhiều nhất = Số nghỉ việc: Số lượt nghỉ việc trong kỳ. = sum({group_hr.f_employee_transaction.is_terminate})

Số lượng nhân sự sắp nghỉ việc = Số NS sẽ nghỉ trong 1–3 tháng tới: Số NS có đơn nghỉ / ngày nghỉ dự kiến trong 1–3 tháng. = distinct_countIf({group_hr.f_resignation_request.employee_key}, dateDiff(${pNgayChot}, {group_hr.f_resignation_request.expected_last_date}, "MM") BETWEEN 0 AND 3)
```

### Đào tạo · PG-GRP-07 · 12 dòng

```
Số giờ đào tạo bình quân đầu người = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({group_hr.f_training.training_hours}) / avg({group_hr.f_training_plan.hc_avg} (trường tính trong Gold))

Số giờ đào tạo bình quân đầu người = % giờ đào tạo so kế hoạch / định mức: Giờ đào tạo thực hiện / giờ kế hoạch (hoặc định mức × headcount). = sum({group_hr.f_training.training_hours}) / sum({group_hr.f_training_plan.planned_hours})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo: Tổng chi phí đào tạo phân bổ (HRM hiện = 0). = sum({group_hr.f_training.training_cost_alloc})

Chi phí đào tạo bình quân đầu người = Chi phí đào tạo / người: Chi phí đào tạo / số NS tham gia đào tạo. = {group_hr.f_training_plan.chi_phi_dao_tao} (trường tính trong Gold) / {group_hr.f_training_plan.so_ns_dao_tao} (trường tính trong Gold)

Số giờ đào tạo khối văn phòng và khối công trường = Số giờ đào tạo: Tổng thời lượng đào tạo của học viên có tham gia. = sum({group_hr.f_training.training_hours})

Số giờ đào tạo khối văn phòng và khối công trường = Giờ đào tạo bình quân / người: Tổng giờ đào tạo / headcount bình quân. = sum({group_hr.f_training.training_hours}) / avg({group_hr.d_training_course.hc_avg} (trường tính trong Gold))

Top 5 giảng viên nội bộ đứng lớp theo quý = Số lớp / giờ đứng lớp của giảng viên nội bộ: Số lớp và giờ đứng lớp theo giảng viên nội bộ, theo quý (Top 5). = distinct_countIf({group_hr.f_training_delivery.class_key}, {group_hr.f_training_delivery.is_internal_instructor} = 1); sum({group_hr.f_training_delivery.teaching_hours})

Tần suất tổ chức các chương trình đào tạo trong năm = Số lớp đào tạo: Số lớp/đợt đào tạo khác nhau (HST yêu cầu đếm chương trình). = distinct_count({group_hr.f_training.class_key})

Số nhân sự tham gia đào tạo theo nhóm chương trình = Số nhân sự tham gia đào tạo: Số nhân viên khác nhau có tham gia ít nhất một lớp. = distinct_countIf({group_hr.f_training.employee_key}, {group_hr.f_training.is_attended} = 1)

Số nhân sự tham gia đào tạo theo nhóm chương trình = Lượt học viên: Số lượt tham gia có điểm danh. = countIf({group_hr.f_training.employee_key}, {group_hr.f_training.is_attended} = 1)

Số nhân sự đang được kèm cặp theo chức danh = Tổng số nhân sự tại ngày chốt: Số người khác nhau được tính headcount tại ngày cuối tháng đã chọn. = distinct_countIf({group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount} = 1 AND {group_hr.f_employee_balance.is_month_end} (trường tính trong Gold) = 1 AND {group_hr.f_employee_balance.snapshot_date} = ${pNgayChot})

Nhân sự chưa có chứng chỉ tương ứng với chức danh = Tỷ lệ NS đủ chứng chỉ bắt buộc theo chức danh: Số NS có đủ chứng chỉ bắt buộc của chức danh / số NS giữ chức danh. = distinct_countIf({group_hr.f_certificate.employee_key}, {group_hr.f_certificate.du_cc_bat_buoc} (trường tính trong Gold) = 1) / distinct_count({group_hr.f_certificate.employee_key})
```

## Thẻ không gắn KPI (không có công thức KPI)

Các thẻ này đếm / nhóm trực tiếp trên cột dữ liệu, không có công thức KPI.

### Cấp công ty · SOL E&C

```
Kế hoạch đào tạo Hệ sinh thái = Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.f_training.class_key}, {group_hr.d_training_course.class_name}, {group_hr.d_training_course.training_type}, {group_hr.d_training_course.start_date}

Giờ đào tạo theo phòng ban khối văn phòng = Giờ đào tạo lũy kế năm của từng phòng ban, kèm giờ / người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.f_training.class_key}, {group_hr.d_training_course.class_name}, {group_hr.d_training_course.training_type}, {group_hr.d_training_course.start_date}

Giờ đào tạo theo dự án / công trường = Giờ đào tạo lũy kế năm của từng dự án, kèm giờ / người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.f_training.class_key}, {group_hr.d_training_course.class_name}, {group_hr.d_training_course.training_type}, {group_hr.d_training_course.start_date}

Hồ sơ năng lực cán bộ quản lý = Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.d_employee.employee_code}, {group_hr.d_employee.employee_name}, {group_hr.d_employee.dob}, {group_hr.d_employee.seniority_start_date}, {group_hr.d_jobtitle.jobtitle_name}, {group_hr.f_manager_assignment.revenue_assigned}, {group_hr.f_manager_assignment.project_key}
```

### Cấp hệ sinh thái · Tổng HST

```
Sơ đồ tổ chức (số trên ô) = Số trên ô = đếm số nhân sự đang làm việc (headcount) của đơn vị và các đơn vị cấp dưới tại ngày chốt, mỗi người đếm 1 lần. = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.f_employee_balance.employee_key}, {group_hr.f_employee_balance.is_headcount}, {group_hr.f_employee_balance.dept_key}, {group_hr.d_department.dept_key}, {group_hr.d_department.parent_dept_key}

Kế hoạch đào tạo Hệ sinh thái = Các chương trình quy mô Hệ sinh thái sắp tổ chức theo kế hoạch đào tạo năm của Ban Đào tạo Hệ sinh thái: thời gian, hình thức, địa điểm, giảng viên, đối tượng, công ty tham gia, đăng ký / chỉ tiêu = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.f_training.class_key}, {group_hr.d_training_course.class_name}, {group_hr.d_training_course.training_type}, {group_hr.d_training_course.start_date}

Hồ sơ năng lực cán bộ quản lý = Bảng Giám đốc điều hành & Giám đốc khối: khối / phòng ban, dự án phụ trách, doanh thu kế hoạch – thực hiện. Hồ sơ từng cán bộ (Tổng Giám đốc, GĐĐH, GĐ Khối, GĐ dự án): phòng ban / dự án phụ trách, doanh thu giao – thực hiện, mentee đang kèm cặp · lọc theo công ty, chức danh rồi tìm và chọn từng người = Không có công thức KPI trong v4; thẻ đếm / nhóm trực tiếp trên {group_hr.d_employee.employee_code}, {group_hr.d_employee.employee_name}, {group_hr.d_employee.dob}, {group_hr.d_employee.seniority_start_date}, {group_hr.d_jobtitle.jobtitle_name}, {group_hr.f_manager_assignment.revenue_assigned}, {group_hr.f_manager_assignment.project_key}
```