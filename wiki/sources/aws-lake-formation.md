---
type: source
title: AWS Lake Formation - Data Lake Security and Governance
created: 2026-11-13
updated: 2026-11-13
sources: []
tags: [aws, lake-formation, security, governance, access-control, data-lake, rbac, tbac]
status: stable
---

# AWS Lake Formation: Data Lake Security and Governance

- **Original Title**: AWS Lake Formation Notes
- **Author / Source**: AWS Documentation & Practical Notes (`raw/aws-lake-formation.md`)
- **Path in raw**: `raw/aws-lake-formation.md`

## Summary

A practical guide to AWS Lake Formation, Amazon's centralized governance and security service for S3 data lakes. The source covers the transition from coarse-grained S3/IAM bucket policies to centralized, SQL-style database, table, column, and row-level access control over the AWS Glue Data Catalog. It details the dual IAM/Lake Formation authorization model, S3 storage location registration with credential vending, Tag-Based Access Control (LF-Tags), cross-account zero-copy data sharing via AWS RAM, and migration from legacy `IAM_ALLOWED_PRINCIPALS`.

## Key Takeaways

- **Centralized Fine-Grained Governance**: Eliminates complex, fragmented S3 bucket policies and IAM roles by centralizing authorization on AWS Glue Data Catalog objects. Provides SQL-style `GRANT` / `REVOKE` permissions down to column-level, row-level, and cell-level granularity (via *data filters*).
- **The Dual Permission Model**: Authorization requires satisfying **both** IAM policies (authorizing access to Lake Formation APIs, Athena, or Glue) and Lake Formation permissions (authorizing access to specific databases, tables, and columns). A failure in either layer results in `Access Denied`.
- **S3 Location Registration & Credential Vending**: Lake Formation assumes an IAM service role to manage registered S3 locations. Query engines (Athena, Glue, Redshift) receive short-lived, temporary S3 credentials via `lakeformation:GetDataAccess`, eliminating the need to grant users direct S3 `s3:GetObject` permissions on raw buckets.
- **LF-Tags (Tag-Based Access Control - TBAC)**: Allows administrators to attach hierarchical key-value tags (e.g., `sensitivity=confidential`, `department=finance`) to catalog resources. Assigning permissions via tag policies scales access control gracefully across large multi-tenant organizations without managing thousands of per-table grants.
- **Legacy Migration Pitfall (`IAM_ALLOWED_PRINCIPALS`)**: Tables created in the Glue Catalog default to granting full access to `IAM_ALLOWED_PRINCIPALS` for backward compatibility. Enforcing Lake Formation security requires explicitly revoking this default permission.
- **Zero-Copy Cross-Account Sharing**: Shares catalog databases and tables across AWS accounts and AWS Organizations via AWS Resource Access Manager (RAM) without replicating physical files on S3.
- **Cost Model**: Lake Formation incurs no separate service fee; billing is derived entirely from the underlying AWS services (S3 storage, Glue jobs/crawlers, and Athena/Redshift query compute).

## Notable Quotes

- *"Giá trị lớn nhất của nó là quản lý quyền truy cập dữ liệu tập trung ở mức chi tiết, thay vì phải tự viết nhiều IAM policy và S3 bucket policy phức tạp."* (Context: Defining the core value proposition of Lake Formation over traditional IAM).
- *"Cần hiểu mối quan hệ giữa IAM permission và Lake Formation permission, vì cả hai cùng được đánh giá. Đây là chỗ người mới dễ bị lỗi 'Access Denied'."* (Context: Highlighting the primary source of authorization failures).
- *"LF-Tags (Tag-based access control): Gắn tag như sensitivity=high hoặc department=finance lên tài nguyên, rồi cấp quyền theo tag. Cách này dễ mở rộng hơn cấp quyền từng bảng."* (Context: Explaining why TBAC is critical for multi-tenant data lakes).

## Pages Created or Updated

- [[lake-formation-security-model]]: Detailed architecture of the dual IAM/Lake Formation model, credential vending, column/row data filters, and LF-Tags TBAC.
- [[lake-formation-access-denied-and-migration]]: Step-by-step diagnostic guide for resolving `Access Denied` errors, revoking `IAM_ALLOWED_PRINCIPALS`, and configuring cross-account RAM shares.
- [[aws-lake-formation-service]]: Platform overview, role within the AWS analytics stack, and supported query engines.
- [[glue-data-catalog]]: Updated to describe how Lake Formation enforces fine-grained authorization over Catalog metadata.
- [[aws-glue-platform]]: Updated to reference Lake Formation as the security governance plane for Glue ETL.

## Related

- [[lake-formation-security-model]]
- [[lake-formation-access-denied-and-migration]]
- [[aws-lake-formation-service]]
- [[glue-data-catalog]]
- [[aws-glue-platform]]
- [[query-io-optimizations]]
