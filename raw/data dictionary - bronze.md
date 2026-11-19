# Data dictionary – SOL E&C – Bronze (BRZ)

Nguồn: `DG-0004_M6_DASHBOARD_SOL_v7.html` (gói DG-0010_GOI_DU_LIEU_v2).

**62 dataset Bronze · 926 cột**

## Mục lục

| Dataset | Công ty | Số cột |
|---|---|---|
| BRZ.SOL.BRAVO.B20ChartOfAccount | SOL | 37 |
| BRZ.SOL.BRAVO.B20Class | SOL | 19 |
| BRZ.SOL.BRAVO.B20ContractType | SOL | 18 |
| BRZ.SOL.BRAVO.B20Currency | SOL | 24 |
| BRZ.SOL.BRAVO.B20Customer | SOL | 77 |
| BRZ.SOL.BRAVO.B20Dept | SOL | 18 |
| BRZ.SOL.BRAVO.B20Employee | SOL | 102 |
| BRZ.SOL.BRAVO.B20ExpenseCatg | SOL | 19 |
| BRZ.SOL.BRAVO.B20Item | SOL | 96 |
| BRZ.SOL.BRAVO.B20ItemGroup | SOL | 35 |
| BRZ.SOL.BRAVO.B20Job | SOL | 24 |
| BRZ.SOL.BRAVO.B20Position | SOL | 20 |
| BRZ.SOL.BRAVO.B20Product | SOL | 81 |
| BRZ.SOL.BRAVO.B20Tax | SOL | 23 |
| BRZ.SOL.BRAVO.B20Territory | SOL | 20 |
| BRZ.SOL.BRAVO.B20Trans | SOL | 39 |
| BRZ.SOL.EXCEL.MAP_9BOX | SOL | 7 |
| BRZ.SOL.EXCEL.MAP_BINS | SOL | 4 |
| BRZ.SOL.EXCEL.MAP_DEPT_TYPE | SOL | 3 |
| BRZ.SOL.EXCEL.MAP_EDUCATION | SOL | 2 |
| BRZ.SOL.EXCEL.MAP_LEAVE_GROUP | SOL | 1 |
| BRZ.SOL.EXCEL.MAP_LEVEL | SOL | 5 |
| BRZ.SOL.EXCEL.MAP_PROJECT_ORG | SOL | 3 |
| BRZ.SOL.EXCEL.MAP_PROVINCE_REGION | SOL | 3 |
| BRZ.SOL.EXCEL.MAP_REGION | SOL | 1 |
| BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT | SOL | 5 |
| BRZ.SOL.EXCEL.MAP_TOP_SCHOOL | SOL | 2 |
| BRZ.SOL.EXCEL.MAP_TRX_TYPE | SOL | 7 |
| BRZ.SOL.EXCEL.MAP_VOLUNTARY | SOL | 2 |
| BRZ.SOL.HRM.CbEmployeeFluctuationDetails | SOL | 7 |
| BRZ.SOL.HRM.CbLeaveReasons | SOL | 1 |
| BRZ.SOL.HRM.CbLeaveRequestDetails | SOL | 9 |
| BRZ.SOL.HRM.CbLeaveRequests | SOL | 5 |
| BRZ.SOL.HRM.CbOrganizations | SOL | 2 |
| BRZ.SOL.HRM.CbXSalaries | SOL | 4 |
| BRZ.SOL.HRM.CbXSalaryDetails | SOL | 31 |
| BRZ.SOL.HRM.CcmToHrmInformations | SOL | 11 |
| BRZ.SOL.HRM.CcmToHrmMonthRevenues | SOL | 4 |
| BRZ.SOL.HRM.ContractLabors | SOL | 6 |
| BRZ.SOL.HRM.EmployeeDetails | SOL | 8 |
| BRZ.SOL.HRM.EmployeeExperiences | SOL | 1 |
| BRZ.SOL.HRM.EmployeeLevels | SOL | 2 |
| BRZ.SOL.HRM.EmployeeWorkProcesses | SOL | 9 |
| BRZ.SOL.HRM.Employees | SOL | 21 |
| BRZ.SOL.HRM.GloneDecisions | SOL | 6 |
| BRZ.SOL.HRM.Glones | SOL | 3 |
| BRZ.SOL.HRM.JobPositions | SOL | 7 |
| BRZ.SOL.HRM.JobTitles | SOL | 4 |
| BRZ.SOL.HRM.ManeuveEmpPlanDetails | SOL | 4 |
| BRZ.SOL.HRM.ManeuveEmpPlanPositions | SOL | 2 |
| BRZ.SOL.HRM.ManeuveEmpPlans | SOL | 1 |
| BRZ.SOL.HRM.OptionValues | SOL | 3 |
| BRZ.SOL.HRM.OrganizationEmployeeNorms | SOL | 18 |
| BRZ.SOL.HRM.Organizations | SOL | 16 |
| BRZ.SOL.HRM.PreferDecisions | SOL | 6 |
| BRZ.SOL.HRM.Schools | SOL | 1 |
| BRZ.SOL.HRM.TalentEvaluationStages | SOL | 2 |
| BRZ.SOL.HRM.TalentEvaluations | SOL | 8 |
| BRZ.SOL.HRM.TerminationDecisions | SOL | 8 |
| BRZ.SOL.HRM.TrainingStageEmployees | SOL | 8 |
| BRZ.SOL.HRM.TrainingStages | SOL | 10 |
| BRZ.SOL.HRM.Wards | SOL | 1 |

## BRZ.SOL.BRAVO.B20ChartOfAccount

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| AccountTypeCode |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ActivityAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankAccountNo |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankCity |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankName |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExpenseCatgAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ForeignCurrAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| GLAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LocalAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LongTermAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Chinese |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Custom |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_English |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_French |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Japanese |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Korean |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OutsideAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductAccount |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20ChartOfAccount; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Class

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Chinese |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Custom |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_English |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_French |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Japanese |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name_Korean |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentCode |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UniqueCode |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Class; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20ContractType

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| BranchCode |  | Bravo dbo.B20ContractType; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code2 |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20ContractType; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Ma_Ct |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PaymentTypeCode |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TaxCode |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20ContractType; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Currency

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DecimalRound |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExchangeRateOperator |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Currency; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name1 |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name1_Chinese |  | Bravo dbo.B20Currency; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name1_Custom |  | Bravo dbo.B20Currency; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name1_English |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name1_Japanese |  | Bravo dbo.B20Currency; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SegmentAccount1 |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SegmentAccount2 |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SegmentAccount3 |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Symbol |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Currency; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Currency; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Customer

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Address |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ApproveSend |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| AuthorizeName |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankAccountNo |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankName |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BecomeCustomerDate |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BecomeLeadDate |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BillingAddress |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Birthday |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CCMBudgetId |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckDebt |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeGroup |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeOld1 |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeOld_Basys |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CompletedApprove |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Country |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerFileNo |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerLevelCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerStatusCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerType |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DescriptionLock |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Email |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EmployeeCode_Salesman |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Fax |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| FilePath |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardDate |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardNo |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardNoPerson0 |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardPlace |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InvestCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsCbNv |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsCustomer |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsLLTCQuanLy |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsNoBLBH |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsNoBLTU |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsNotActiveUNC |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsSCRelation |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsStopWorking |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemCatgCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LLTCLevelCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| List_BranchCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Notes |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PeriodPaymentCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Person |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Person0 |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PersonPosition |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PersonTel |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Position |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProcessCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductCostId |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Represent |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ShortName |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SourceLeadCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| StopWorkingDate |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SubContractPay |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TaxRegNo |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Tax_Identification |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Tel |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TerritoryCode |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| VoucherCode |  | Bravo dbo.B20Customer; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Customer; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Dept

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| BranchCode |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code1 |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Dept; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Dept; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Employee

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN nhạy cảm · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ADAddress |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Address |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| AddressSI |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Area |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| AreaCode |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| AutoCreateUser |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Award |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankAccount |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BankName |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BirthDate |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BirthPlace |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BloodType |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BooksType |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClosingDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Ct_Lookup |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerCode |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DCAddress |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DeptCode |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DischargeArmyDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Discipline |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Domicile |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DomicileSI |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Email |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Ethnicity |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN nhạy cảm | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| FileNo |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| FirstWorkingDate |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Gender |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HCAddress |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Health |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN nhạy cảm | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HealthInsuranceNo |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HomeCountry |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HomeCountrySI |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardDate |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardIssuePlace |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IdCardNo |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InsuranceNo |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsLoginWeb |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsUInsurance |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Job |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| JoinArmyDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| JoinPartyDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LastJob |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LongestJob |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MarriageStatus |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Mobile |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MonthsUInsurance |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Nationality |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OfficalJoinPartyDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Origin |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PaidEndDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PaidStartDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PartyCardNo |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PersonalTaxCode |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PolicyBeneficiaries |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RankArmy |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ReceivedBooksDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ReceivedBooksNoDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ReceivedReturnBooksDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Religion |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN nhạy cảm | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Residence |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ResignDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_ADCommune |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_ADDistrict |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_ADProvince |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_CulturalLevel |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_DCCommune |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_DCDistrict |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_DCProvince |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_Ethnicity |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN nhạy cảm | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_HCCommune |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_HCDistrict |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_HCProvince |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_HRMRP |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_Hospital |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_IdCardIssuePlace |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId_Nationality |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalariesUInsurance |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SendingBooksDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ShortName |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Skill |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SocialSecurityDate |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SocialSecurityNo |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Tel |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UserIdGroup |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UserName |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Employee; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Employee; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20ExpenseCatg

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| BranchCode |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DinamoCode |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ListAccount |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20ExpenseCatg; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Item

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| AbrasionCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Area |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BarCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CalPriceByExchange |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CategoryCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code1 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeChung |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ColorList |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Comment |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRate0 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRate1 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRate2 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRateTB |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CustomerCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DiameterIn |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DiameterOut |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EmissionsCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EnvCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EnvTaxRate |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExciseTaxCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExciseTaxRate |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExpandInfo |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| GenusList |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Height |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ImagePath |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InfoTM1 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InfoTM2 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsChkTBNH |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsEquipmentHeavy |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithInstall |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithLocation |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithLot |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithScale |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithSerialNo |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsItemWithTransport |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsReport |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsShowMenuWeb |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsStopBusiness |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemCatgCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemGroupCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemGroupType |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemType |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| JobCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Length |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ManufacturerCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MaxCloseQty |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MinCloseQty |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OriginCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PackagingConvertRate |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PackagingItemCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PackagingUnit |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PercentEfficiency |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PhiThep |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PrefixOrder |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductCostId |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductItemCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProportionCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PurchaseNumDayLimit |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| QuyCach |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RequireAttribute |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesTaxCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ShadeColor |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ShelfLife |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SizeCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SizeList |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SpeciesCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SupProductCode |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SurfaceCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TradeMarkCode |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Unit |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Unit0 |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Unit1 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Unit2 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UnitCost |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UnitIn |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UnitPrice |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| UnitTB |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Weight |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Width |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Item; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Item; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20ItemGroup

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| BranchCode |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| COGSAccount |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| COGSAccount1 |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| COGSAccount2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| COGSAccount3 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemAccount |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemAccount1 |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemAccount2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemAccount3 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemCatgCode |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesAccount |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesAccount1 |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesAccount2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesAccount3 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesDiscountAccount |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesDiscountAccount1 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesDiscountAccount2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesDiscountAccount3 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesReturnAccount |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesReturnAccount1 |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesReturnAccount2 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SalesReturnAccount3 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20ItemGroup; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20ItemGroup; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Job

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ActivityCode |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Job; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CategoryCode |  | Bravo dbo.B20Job; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CategoryCodeOld |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExpenseCatgCode |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsFollowPrice |  | Bravo dbo.B20Job; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Job; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsPurchaseOrder |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsQLKL |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| JobType |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentCode |  | Bravo dbo.B20Job; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Remark |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ShortName |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Job; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Position

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ApproveBehalf |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ApproveSend |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ApproveVeto |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CloseProcess |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeOld |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DeptCode |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsAttachedRule |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Position; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Position; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Product

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Address |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Advisory |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ApproveSend |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code2 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CodeOld_basys |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CompletedApprove |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRate1 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ConvertRate2 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DayOfWarranty |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DeptCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DinamoCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DocNo_Prefix |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EndDateBizDoc |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| EstimateCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExpectedDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ExpenseCatgCode |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| FinishDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| GeneralProject |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| GuidId |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HasContractRule |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| HasLetter |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InvestorCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| InvestorGroupCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsAdj |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsCoteccons |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsEquipmentProduct |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LaborCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| LiquidDate |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MachineryCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MaterialCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MemberCode |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| MergePackage |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OrgStructId |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OtherCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OverheadCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PackageTypeCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentRowId |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProcessCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductCodeGroup |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductCostId0 |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProductType |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProjectContractType |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProjectInfo |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProjectManager |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProjectStatus |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ProjectTypeCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Revenue |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| RowId |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SettlementAmount |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu – NHẠY CẢM (chỉ đếm) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| SettlementDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| StageCode |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| StartDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| StartDateBizDoc |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Stt |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TenderTrasferDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TerritoryCode |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TotalCost |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Unit |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| WarrantyAmount |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| WarrantyDate |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| WarrantyPercent |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| WarrantyStartDate |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Year |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentRowId0 |  | Bravo dbo.B20Product; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Product; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Tax

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Account |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Tax; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Tax; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Deduction |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsDefault |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| PriceIncludeTax |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Rate |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Type |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Tax; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Territory

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| BranchCode |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Range |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TerriCode |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ZipCode |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Territory; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Territory; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.BRAVO.B20Trans

- **Công ty:** SOL · **Loại bảng:** Bronze (danh mục Bravo) · **Hệ thống:** BRAVO
- **Mô tả:** Bảng danh mục Bravo trong file SOL E&C_FIN (kiểm kê M7)
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Account |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchCode |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| BranchSegmentType |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckBizDocId_C1 |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckBizDocId_C2 |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckBizDocId_PO |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckBizDocId_SO |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckDeptCode |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CheckExpenseCatgCode |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode1 |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode2 |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ClassCode3 |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Code |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedAt |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreatedBy |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CreditAccount |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| CurrencySegmentType |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DebitAccount |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| DocGroup |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Id |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsActive |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsGroup |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsImport |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsOutsideLedger |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsPostGL |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| IsPrepayment |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ItemAccountType |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedAt |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ModifiedBy |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Name2 |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| Number_Insterest |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (toàn 0/mặc định) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| OutsideAccount |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| ParentId |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TaxCode |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TransType |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| TransTypeCode |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| _ParentCode0 |  | Bravo dbo.B20Trans; kiểm kê: Chỉ cấu trúc (rỗng) | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |
| timestamp |  | Bravo dbo.B20Trans; kiểm kê: Có dữ liệu mẫu | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/out/DG-0004_M7_FIN_KIEM_KE_v3.xlsx › Kiem_ke |

## BRZ.SOL.EXCEL.MAP_9BOX

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Box_Key |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Box_Name |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Description |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Perf_From |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Perf_To |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Potential_From |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |
| Potential_To |  | Cột nguồn EXCEL MAP_9BOX | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_9BOX; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_9BOX) |

## BRZ.SOL.EXCEL.MAP_BINS

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Bin_Name |  | Cột nguồn EXCEL MAP_BINS | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_BINS; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_BINS) |
| From_Value |  | Cột nguồn EXCEL MAP_BINS | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_BINS; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_BINS) |
| Sort |  | Cột nguồn EXCEL MAP_BINS | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_BINS; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_BINS) |
| To_Value |  | Cột nguồn EXCEL MAP_BINS | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_BINS; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_BINS) |

## BRZ.SOL.EXCEL.MAP_DEPT_TYPE

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Dept_Type |  | Cột nguồn EXCEL MAP_DEPT_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_DEPT_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_DEPT_TYPE) |
| Is_Headcount |  | Cột nguồn EXCEL MAP_DEPT_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_DEPT_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_DEPT_TYPE) |
| Short_Name |  | Cột nguồn EXCEL MAP_DEPT_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_DEPT_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_DEPT_TYPE) |

## BRZ.SOL.EXCEL.MAP_EDUCATION

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Education |  | Cột nguồn EXCEL MAP_EDUCATION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_EDUCATION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_EDUCATION) |
| Education_Group |  | Cột nguồn EXCEL MAP_EDUCATION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_EDUCATION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_EDUCATION) |

## BRZ.SOL.EXCEL.MAP_LEAVE_GROUP

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Leave_Group |  | Cột nguồn EXCEL MAP_LEAVE_GROUP | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEAVE_GROUP; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEAVE_GROUP) |

## BRZ.SOL.EXCEL.MAP_LEVEL

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Level_Group |  | Cột nguồn EXCEL MAP_LEVEL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEVEL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEVEL) |
| Level_Manager_Group |  | Cột nguồn EXCEL MAP_LEVEL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEVEL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEVEL) |
| Level_Manager_Sub_Group |  | Cột nguồn EXCEL MAP_LEVEL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEVEL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEVEL) |
| Level_Order |  | Cột nguồn EXCEL MAP_LEVEL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEVEL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEVEL) |
| Similar_Level |  | Cột nguồn EXCEL MAP_LEVEL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_LEVEL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_LEVEL) |

## BRZ.SOL.EXCEL.MAP_PROJECT_ORG

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Organization_Id |  | Cột nguồn EXCEL MAP_PROJECT_ORG | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROJECT_ORG; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROJECT_ORG) |
| Project_Code |  | Cột nguồn EXCEL MAP_PROJECT_ORG | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROJECT_ORG; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROJECT_ORG) |
| Revenue_Alloc_Pct |  | Cột nguồn EXCEL MAP_PROJECT_ORG | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROJECT_ORG; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROJECT_ORG) |

## BRZ.SOL.EXCEL.MAP_PROVINCE_REGION

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Province_Group |  | Cột nguồn EXCEL MAP_PROVINCE_REGION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROVINCE_REGION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROVINCE_REGION) |
| Province_Name |  | Cột nguồn EXCEL MAP_PROVINCE_REGION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROVINCE_REGION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROVINCE_REGION) |
| Region_Code |  | Cột nguồn EXCEL MAP_PROVINCE_REGION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_PROVINCE_REGION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_PROVINCE_REGION) |

## BRZ.SOL.EXCEL.MAP_REGION

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Region_Code |  | Cột nguồn EXCEL MAP_REGION | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_REGION; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_REGION) |

## BRZ.SOL.EXCEL.MAP_SALARY_COMPONENT

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Component_Code |  | Cột nguồn EXCEL MAP_SALARY_COMPONENT | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_SALARY_COMPONENT; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_SALARY_COMPONENT) |
| Component_Group |  | Cột nguồn EXCEL MAP_SALARY_COMPONENT | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_SALARY_COMPONENT; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_SALARY_COMPONENT) |
| Component_Name |  | Cột nguồn EXCEL MAP_SALARY_COMPONENT | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_SALARY_COMPONENT; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_SALARY_COMPONENT) |
| Is_Employee_Cost |  | Cột nguồn EXCEL MAP_SALARY_COMPONENT | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_SALARY_COMPONENT; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_SALARY_COMPONENT) |
| Parent_Name |  | Cột nguồn EXCEL MAP_SALARY_COMPONENT | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_SALARY_COMPONENT; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_SALARY_COMPONENT) |

## BRZ.SOL.EXCEL.MAP_TOP_SCHOOL

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Top_School_Group |  | Cột nguồn EXCEL MAP_TOP_SCHOOL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TOP_SCHOOL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TOP_SCHOOL) |
| Top_School_Order |  | Cột nguồn EXCEL MAP_TOP_SCHOOL | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TOP_SCHOOL; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TOP_SCHOOL) |

## BRZ.SOL.EXCEL.MAP_TRX_TYPE

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Is_Appointed |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Is_Grade_Change |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Is_Rehire |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Is_Transfer |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Status_After |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Trx_Group |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |
| Trx_Type |  | Cột nguồn EXCEL MAP_TRX_TYPE | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_TRX_TYPE; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_TRX_TYPE) |

## BRZ.SOL.EXCEL.MAP_VOLUNTARY

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** EXCEL
- **Mô tả:** Bảng mapping nhập tay (sheet MAP_* file thiết kế)
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Resignation_Type |  | Cột nguồn EXCEL MAP_VOLUNTARY | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_VOLUNTARY; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_VOLUNTARY) |
| Voluntary |  | Cột nguồn EXCEL MAP_VOLUNTARY | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/SOL_HR_GOLD_SEMANTIC_DESIGN_v1.xlsx › MAP_VOLUNTARY; DG-0004/out/SOL_HR_GOLD_DDL_v3.sql (LND.SOL_MNL_VOLUNTARY) |

## BRZ.SOL.HRM.CbEmployeeFluctuationDetails

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| EmployeeId | uniqueidentifier | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiBienDong | int | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Ngay | date | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TG_Den | datetime | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TG_Tu | datetime | Cột nguồn HRM CbEmployeeFluctuationDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbLeaveReasons

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Name | nvarchar(500) | Cột nguồn HRM CbLeaveReasons | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbLeaveRequestDetails

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DenNgay | datetime | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LyDoNghi | int | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| RequestId | int | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoNgayDangKyNghi | float | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoNgayPhepCheDo | float | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoNgayPhepCoLuong | float | Cột nguồn HRM CbLeaveRequestDetails (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoNgayPhepKhongLuong | float | Cột nguồn HRM CbLeaveRequestDetails (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TuNgay | datetime | Cột nguồn HRM CbLeaveRequestDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbLeaveRequests

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| EmployeeId | uniqueidentifier | Cột nguồn HRM CbLeaveRequests | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM CbLeaveRequests | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM CbLeaveRequests | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM CbLeaveRequests | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Status | int | Cột nguồn HRM CbLeaveRequests | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbOrganizations

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| MaHachToan | varchar(50) | Cột nguồn HRM CbOrganizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM CbOrganizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbXSalaries

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Id | int | Cột nguồn HRM CbXSalaries | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Month | int | Cột nguồn HRM CbXSalaries | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayThanhToan | date | Cột nguồn HRM CbXSalaries | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Year | int | Cột nguồn HRM CbXSalaries | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CbXSalaryDetails

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| CbOrganizationId | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| GloneId | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobTitleId | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiBoPhan | varchar(50) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NhomNhanSu | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD003 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD018 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD019 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD020 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD021 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD022 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD023 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD025 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD026 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD028 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD039 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD040 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD041 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD042 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD043 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD044 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD045 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD046 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD067 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD083 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD094 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD104 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SD105 | decimal(18, 0) | Cột nguồn HRM CbXSalaryDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CcmToHrmInformations

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DoanhThu_HDCDT | decimal(18, 0) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DoanhThu_KeHoach | decimal(18, 0) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DoanhThu_ThucTe | decimal(18, 0) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiHinhDuAn | nvarchar(MAX) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| MaDuAn | varchar(200) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| MaVietTat | varchar(500) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayBatDau | datetime | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayKetThuc | datetime | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoThang | int | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TenDuAn | nvarchar(MAX) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TrangThai | nvarchar(MAX) | Cột nguồn HRM CcmToHrmInformations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.CcmToHrmMonthRevenues

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DT_ThucTe | decimal(18, 0) | Cột nguồn HRM CcmToHrmMonthRevenues | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| MaDuAn | varchar(500) | Cột nguồn HRM CcmToHrmMonthRevenues | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Month | int | Cột nguồn HRM CcmToHrmMonthRevenues | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Year | int | Cột nguồn HRM CcmToHrmMonthRevenues | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.ContractLabors

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM ContractLabors | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM ContractLabors | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM ContractLabors | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NhomHD | int | Cột nguồn HRM ContractLabors | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TG_Den | date | Cột nguồn HRM ContractLabors | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TG_Tu | date | Cột nguồn HRM ContractLabors | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.EmployeeDetails

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| NamBatDauDiLam | date | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NoiDaoTao | int | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NoiTuyenDung | int | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThuongTru_Duong | nvarchar(500) | Cột nguồn HRM EmployeeDetails (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThuongTru_Phuong | int | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThuongTru_Tinh | int | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThuongTru_Tinh_Old | int | Cột nguồn HRM EmployeeDetails | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| VungMien | int | Cột nguồn HRM EmployeeDetails | Thực |  | Chờ xác nhận | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.EmployeeExperiences

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ThoiGian_Tu | varchar(50) | Cột nguồn HRM EmployeeExperiences | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.EmployeeLevels

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM EmployeeLevels | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Name | nvarchar(250) | Cột nguồn HRM EmployeeLevels | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.EmployeeWorkProcesses

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DonVi_Cu | int | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EndDate | datetime | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OwnerId | uniqueidentifier | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QuyetDinh_Loai | int | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QuyetDinh_So | varchar(50) | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| StartDate | datetime | Cột nguồn HRM EmployeeWorkProcesses | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.Employees

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ChuyenNganh | int | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DiemGoc | int | Cột nguồn HRM Employees | Thực |  | Chờ xác nhận | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Email | nvarchar(256) | Cột nguồn HRM Employees (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmailCongTy | varchar(250) | Cột nguồn HRM Employees (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| FullName | nvarchar(256) | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| GioiTinh | int | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| GloneId | int | Cột nguồn HRM Employees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | uniqueidentifier | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LastOrgId | int | Cột nguồn HRM Employees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LastPostId | int | Cột nguồn HRM Employees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| MEP | bit | Cột nguồn HRM Employees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayNghiViec | date | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgaySinh | date | Cột nguồn HRM Employees (DS_COT đánh dấu nhạy cảm) | Thực |  | Chờ xác nhận | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayVaoCongTy | date | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayVaoCongTyChinhThuc | date | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayVaoGroup | date | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| StaffCode | varchar(250) | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThoiGianKhongTinhThamNien | int | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TinhTrangHonNhan | int | Cột nguồn HRM Employees (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TrangThaiCongViec | int | Cột nguồn HRM Employees | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TrinhDoDaoTao | int | Cột nguồn HRM Employees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.GloneDecisions

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM GloneDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM GloneDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM GloneDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM GloneDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Ngach_Moi | int | Cột nguồn HRM GloneDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayHieuLuc | date | Cột nguồn HRM GloneDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.Glones

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(10) | Cột nguồn HRM Glones | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM Glones | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Status | int | Cột nguồn HRM Glones | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.JobPositions

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Active | bit | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Code | varchar(50) | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| GroupEmployeeId | int | Cột nguồn HRM JobPositions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobTitleId | int | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LevelId | int | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Name | nvarchar(250) | Cột nguồn HRM JobPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.JobTitles

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM JobTitles | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM JobTitles | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Name | nvarchar(250) | Cột nguồn HRM JobTitles | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Priority | int | Cột nguồn HRM JobTitles | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.ManeuveEmpPlanDetails

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Month | int | Cột nguồn HRM ManeuveEmpPlanDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| PlanPositionId | int | Cột nguồn HRM ManeuveEmpPlanDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoLuong | int | Cột nguồn HRM ManeuveEmpPlanDetails (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Year | int | Cột nguồn HRM ManeuveEmpPlanDetails | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.ManeuveEmpPlanPositions

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| JobPositionId | int | Cột nguồn HRM ManeuveEmpPlanPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| PlanId | int | Cột nguồn HRM ManeuveEmpPlanPositions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.ManeuveEmpPlans

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| OrganizationId | int | Cột nguồn HRM ManeuveEmpPlans | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.OptionValues

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM OptionValues | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Name | nvarchar(MAX) | Cột nguồn HRM OptionValues | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Type | varchar(50) | Cột nguồn HRM OptionValues | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.OrganizationEmployeeNorms

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Cumulative_Salary | nvarchar(MAX) | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DoanhThu | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DuPhong_BCH | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EndDate | datetime | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiHinhDuAn | int | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NangXuatLaoDong | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NangXuatLaoDong_ToiThieu | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Percent_QL_DoanhThu | float | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Percent_QuyLuongToiDa | float | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QuyLuongKeHoach | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QuyLuongToiDa | decimal(18, 0) | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QuyMoDuAn | nvarchar(MAX) | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Salary_Plan | nvarchar(MAX) | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| SoThang | int | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| StartDate | datetime | Cột nguồn HRM OrganizationEmployeeNorms | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TongSoLuongNhanSu | int | Cột nguồn HRM OrganizationEmployeeNorms (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.Organizations

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| CountryId | int | Cột nguồn HRM Organizations | Thực |  | Chờ xác nhận | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| GiaTriDuAn | decimal(18, 0) | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| InvestorId | int | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| KhuVuc | int | Cột nguồn HRM Organizations | Thực |  | Chờ xác nhận | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiHinhDuAn | int | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Name | nvarchar(250) | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayThanhLap | date | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrgLevelId | int | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ParentId | int | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Priority | float | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Stopped | bit | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TGTC_Den | date | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TGTC_Tu | date | Cột nguồn HRM Organizations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Type | int | Cột nguồn HRM Organizations | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.PreferDecisions

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(50) | Cột nguồn HRM PreferDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM PreferDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM PreferDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM PreferDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayHieuLuc | date | Cột nguồn HRM PreferDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM PreferDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.Schools

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Name | nvarchar(250) | Cột nguồn HRM Schools | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.TalentEvaluationStages

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Không phải DLCN · **Mức bảo mật đề xuất:** Nội bộ

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Month | int | Cột nguồn HRM TalentEvaluationStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Year | int | Cột nguồn HRM TalentEvaluationStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.TalentEvaluations

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DiemTongKet | varchar(50) | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM TalentEvaluations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NS_BoNhiem | bit | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NS_PhanLoaiNhanSu | int | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QL_YKien_BoNhiem | bit | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| QL_YKien_PhanLoai | int | Cột nguồn HRM TalentEvaluations | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| StageId | int | Cột nguồn HRM TalentEvaluations | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.TerminationDecisions

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** Chờ xác nhận · **Mức bảo mật đề xuất:** Đặc biệt bảo mật

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Code | varchar(250) | Cột nguồn HRM TerminationDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM TerminationDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| HinhThuc | int | Cột nguồn HRM TerminationDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM TerminationDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LyDo | int | Cột nguồn HRM TerminationDecisions | Thực |  | Chờ xác nhận | Đặc biệt bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayHieuLuc | date | Cột nguồn HRM TerminationDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM TerminationDecisions | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| RequestId | int | Cột nguồn HRM TerminationDecisions | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.TrainingStageEmployees

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| DiemDanh_TrangThai | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| EmployeeId | uniqueidentifier | Cột nguồn HRM TrainingStageEmployees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| JobPositionId | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| KetQua | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| OrganizationId | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Test_KetQua | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TinhTrang | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TrainingStageId | int | Cột nguồn HRM TrainingStageEmployees | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.TrainingStages

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| ChiPhi | decimal(18, 0) | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Bảo mật |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Code | varchar(500) | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| DiaDiem | nvarchar(500) | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| HinhThucDaoTao | int | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Id | int | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| LoaiHinhDaoTao | int | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| NgayKhaiGiang | date | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| Status | int | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| TenChuongTrinh | nvarchar(500) | Cột nguồn HRM TrainingStages | Thực |  | Không phải DLCN | Nội bộ |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
| ThoiLuong | int | Cột nguồn HRM TrainingStages (DS_COT đánh dấu nhạy cảm) | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |

## BRZ.SOL.HRM.Wards

- **Công ty:** SOL · **Loại bảng:** Bronze · **Hệ thống:** HRM
- **Mô tả:** Bảng nguồn HRM
- **Nhãn nguồn:** Thực · **DLCN:** DLCN cơ bản · **Mức bảo mật đề xuất:** Hạn chế

| Cột | Kiểu | Mô tả | Nhãn nguồn | Phân loại Owner | DLCN | Mức bảo mật | Dùng ở Gold | Căn cứ |
|---|---|---|---|---|---|---|---|---|
| Name | nvarchar(200) | Cột nguồn HRM Wards | Thực |  | DLCN cơ bản | Hạn chế |  | DG-0004/src/OUTPUT/00_KIEM_KE_CAU_TRUC_HRM.xlsx › DS_COT; DG-0004/src/DDL-HRM.sql |
