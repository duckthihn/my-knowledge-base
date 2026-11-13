---
type: concept
title: Lake Formation Security Model and Access Control
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-lake-formation]]", "[[aws-glue]]"]
tags: [aws, lake-formation, security, iam, access-control, rbac, tbac, credential-vending]
status: stable
---

# Lake Formation Security Model and Access Control

AWS Lake Formation replaces fragmented, coarse-grained S3 bucket policies and IAM roles with a centralized, table-aware authorization engine layered over the [[glue-data-catalog|AWS Glue Data Catalog]] (see [[aws-lake-formation]]). It enforces security across two distinct authorization layers (IAM and Lake Formation), manages access via short-lived credential vending, provides granular row- and column-level data filtering, and enables scalable governance through Tag-Based Access Control (LF-Tags) (see [[aws-lake-formation]]).

## The Dual Permission Model

In Lake Formation, access evaluation is strictly additive: a principal must satisfy **both** IAM policies and Lake Formation permissions to query or modify data (see [[aws-lake-formation]]):

```
User Query (e.g. Athena, Glue, Redshift)
   │
   ▼
[1. IAM Permission Check]
   Does principal have IAM permission to call:
   - athena:StartQueryExecution?
   - glue:GetDatabase, glue:GetTable?
   - lakeformation:GetDataAccess?
   │
   ├── DENY ──> Immediate "Access Denied" (IAM failure)
   └── ALLOW
       │
       ▼
[2. Lake Formation Permission Check]
   Does principal have Lake Formation GRANT on:
   - Target Database? (DESCRIBE)
   - Target Table? (SELECT)
   - Specific Columns & Data Filters (Row-level)?
   │
   ├── DENY ──> "Access Denied: Insufficient Lake Formation permissions"
   └── ALLOW
       │
       ▼
[3. Credential Vending & Execution]
   Lake Formation returns scoped, temporary S3 credentials to engine
   Engine reads only authorized rows and columns
```

1. **IAM Policy Layer**:
   - Governs the control plane and API execution rights (see [[aws-lake-formation]]).
   - Verifies whether the calling IAM user, role, or SAML identity is permitted to invoke query engines (e.g., `athena:*`), read catalog metadata (`glue:Get*`), and request credentials from Lake Formation (`lakeformation:GetDataAccess`) (see [[aws-lake-formation]]).
2. **Lake Formation Policy Layer**:
   - Governs catalog resources and data slices using SQL-style `GRANT` / `REVOKE` semantics (see [[aws-lake-formation]]).
   - Verifies whether the identity has explicit rights on the target database, table, columns, or associated LF-Tags (see [[aws-lake-formation]]).

## Storage Registration and Credential Vending

Under traditional IAM architectures, users require direct `s3:GetObject` permissions on raw S3 bucket prefixes, creating severe risks of accidental data leakage or bypassing metadata filters (see [[aws-lake-formation]]):

- **Data Lake Location Registration**:
  - Administrators register target S3 bucket locations (e.g., `s3://company-lakehouse/curated/`) with Lake Formation (see [[aws-lake-formation]]).
  - Registration associates an IAM service role that possesses read and write permissions to the underlying S3 storage (see [[aws-lake-formation]]).
- **Dynamic Credential Vending**:
  - When an authorized query runs in Amazon Athena or AWS Glue, the engine calls Lake Formation's `GetDataAccess` API (see [[aws-lake-formation]]).
  - Lake Formation validates permissions and dynamically vends **temporary, scoped AWS STS credentials** directly to the query engine (see [[aws-lake-formation]]).
  - **Security Guarantee**: End users and analysts never possess direct S3 read permissions; all physical data access is mediated and bounded by Lake Formation (see [[aws-lake-formation]]).

## Data Filters: Column-, Row-, and Cell-Level Security

Lake Formation enforces fine-grained access control without duplicating physical tables or creating complex database views (see [[aws-lake-formation]]):

1. **Column-Level Security**:
   - Administrators configure column inclusion or exclusion masks (see [[aws-lake-formation]]).
   - *Example*: Granting the marketing team access to all customer fields except `ssn`, `credit_card`, and `salary` (see [[aws-lake-formation]]).
2. **Row-Level Security (Data Filters)**:
   - Uses SQL `WHERE` clauses to restrict which rows a principal can see (see [[aws-lake-formation]]).
   - *Example*: A data filter defined as `country = 'US'` ensures US analysts see only domestic records, while `country = 'UK'` partitions access for European analysts on the identical physical table (see [[aws-lake-formation]]).
3. **Cell-Level Security**:
   - The intersection of column masking and row filtering, allowing organizations to restrict both vertical and horizontal data slices simultaneously (see [[aws-lake-formation]]).

## Tag-Based Access Control (LF-Tags)

Managing permissions at the individual table and column level becomes unmanageable as data lakes scale to thousands of tables and hundreds of users (see [[aws-lake-formation]]):

```
                     ┌─────────────────────────────┐
                     │ LF-Tag: Sensitivity=Confidential │
                     └──────────────┬──────────────┘
                                    │ Attached to
                     ┌──────────────▼──────────────┐
                     │  orders_table.credit_card   │
                     └─────────────────────────────┘
                                    ▲
                                    │ Allowed via Policy
┌───────────────────────────────┐   │
│ Role: Finance-Compliance-Role ├───┘
│ Policy: ALLOW Sensitivity IN (Public, Confidential)
└───────────────────────────────┘
```

- **Tag Definition**: LF-Tags are key-value pairs defined in Lake Formation (e.g., `Sensitivity = [Public, Internal, Confidential, Restricted]` or `Department = [Finance, HR, Sales]`) (see [[aws-lake-formation]]).
- **Hierarchical Inheritance**:
  - Tags applied to a **Database** automatically cascade down to all current and future **Tables** within that database (see [[aws-lake-formation]]).
  - Tags applied to a **Table** cascade to all its **Columns**, unless specifically overridden at the column tier (see [[aws-lake-formation]]).
- **Policy Scaling Advantage**:
  - Instead of executing $N \times M$ direct grants (users $\times$ tables), administrators assign LF-Tag grant expressions to IAM roles (e.g., *Grant SELECT on all resources tagged `Department=Finance` AND `Sensitivity=Internal`*) (see [[aws-lake-formation]]).
  - When a new table or column is tagged, authorized roles inherit access instantaneously without modifying IAM or table policies, reducing policy overhead from $O(N \times M)$ to $O(1)$ (see [[aws-lake-formation]]).

## Related

- [[aws-lake-formation]]
- [[lake-formation-access-denied-and-migration]]
- [[aws-lake-formation-service]]
- [[glue-data-catalog]]
- [[aws-glue-platform]]
- [[query-io-optimizations]]
