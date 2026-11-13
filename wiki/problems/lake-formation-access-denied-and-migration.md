---
type: problem
title: Troubleshooting Lake Formation Access Denied and IAM Migration
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-lake-formation]]", "[[aws-glue]]"]
tags: [aws, lake-formation, troubleshooting, access-denied, iam, migration, security, cross-account]
status: stable
---

# Troubleshooting Lake Formation Access Denied and IAM Migration

Because AWS Lake Formation layers granular table-level controls on top of standard AWS Identity and Access Management (IAM), authorization errors (most notably `Access Denied: Insufficient Lake Formation permissions` or `Access Denied on S3`) are frequent during implementation and migration (see [[aws-lake-formation]], [[lake-formation-security-model]]). Resolving these issues requires systematic evaluation of the dual permission model, storage location registration, and legacy `IAM_ALLOWED_PRINCIPALS` overrides (see [[aws-lake-formation]]).

## The 4-Step "Access Denied" Diagnostic Tree

When a query in Amazon Athena, AWS Glue, or Amazon Redshift fails with an authorization error, execute this diagnostic checklist in order (see [[aws-lake-formation]], [[lake-formation-security-model]]):

```
Query Fails with "Access Denied"
   │
   ├── Step 1: Check Principal's IAM Policy
   │   └── Does the IAM Role have athena:*, glue:Get*, and lakeformation:GetDataAccess?
   │       └── NO ──> Fix IAM Role Policy
   │
   ├── Step 2: Check Lake Formation Catalog Grants
   │   └── Does the IAM Role have DESCRIBE on DB and SELECT on Table/Columns?
   │       └── NO ──> Grant permissions via LF console or LF-Tag policy
   │
   ├── Step 3: Check S3 Data Lake Location Registration
   │   └── Is S3 path registered under "Data Lake Locations"?
   │       Does the registration role have s3:GetObject / s3:PutObject permissions?
   │       └── NO ──> Register location & fix registration service role
   │
   └── Step 4: Check S3 Bucket Policies and AWS KMS Key Policies
       └── Does an explicit S3 Bucket Policy DENY block the Lake Formation role?
           Does the KMS Key policy permit the registration role to call kms:Decrypt?
           └── YES ──> Update S3 bucket and KMS key policies
```

### Step 1: Verify IAM Control Plane Permissions
The calling IAM user or role must possess standard IAM permissions to call engine and catalog control plane APIs (see [[aws-lake-formation]]):
- `athena:StartQueryExecution`, `athena:GetQueryExecution`, `athena:GetQueryResults`
- `glue:GetDatabase`, `glue:GetDatabases`, `glue:GetTable`, `glue:GetPartitions`
- `lakeformation:GetDataAccess` (required for credential vending) (see [[lake-formation-security-model]])
*Crucial*: The calling principal does **not** need direct S3 `s3:GetObject` permissions on the registered data bucket (see [[aws-lake-formation]]).

### Step 2: Verify Lake Formation Catalog Grants
Confirm that the exact IAM principal (or an assumed role session) holds Lake Formation grants (see [[aws-lake-formation]]):
- **Database Level**: The principal must hold `DESCRIBE` on the parent catalog database (see [[aws-lake-formation]]).
- **Table Level**: The principal must hold `SELECT` on the target table (or columns) (see [[aws-lake-formation]]).
- **Data Filters**: If row-level filtering is applied, verify that the filter condition does not evaluate to false for all rows (which returns an empty dataset or error depending on query semantics) (see [[lake-formation-security-model]]).

### Step 3: Verify S3 Location Registration & Service Role
Lake Formation mediates S3 access by assuming a designated IAM role when vending credentials (see [[aws-lake-formation]], [[lake-formation-security-model]]):
- In the Lake Formation console, verify that the target S3 prefix (e.g., `s3://lakehouse-bucket/data/`) is listed under **Data lake locations** (see [[aws-lake-formation]]).
- Inspect the registered IAM service role (commonly `AWSServiceRoleForLakeFormationDataAccess` or a custom role):
  - Ensure its trust policy allows `lakeformation.amazonaws.com` to assume the role.
  - Ensure its permission policy includes `s3:GetObject*`, `s3:PutObject*`, `s3:ListBucket` on the target bucket.

### Step 4: Audit S3 Bucket Policies and KMS Encryption
Even if Lake Formation permissions are pristine, underlying storage controls can enforce hard blocks (see [[aws-lake-formation]]):
- **Explicit Denies in Bucket Policies**: S3 evaluates bucket policies alongside IAM. An explicit `Deny` (e.g., restricting access to specific VPC endpoints or IP ranges) will override Lake Formation's assumed role credentials.
- **KMS Key Access**: If S3 data files are encrypted with a Customer Managed Key (AWS KMS SSE-KMS), the Lake Formation registration role must be granted `kms:Decrypt`, `kms:GenerateDataKey`, and `kms:DescribeKey` in the KMS key policy (see [[aws-lake-formation]]).

## The `IAM_ALLOWED_PRINCIPALS` Migration Trap

For backward compatibility with traditional Hive/IAM architectures, AWS Glue automatically creates new databases and tables with an implicit grant to a pseudo-principal named **`IAM_ALLOWED_PRINCIPALS`** (see [[aws-lake-formation]]):

- **The Problem**: As long as `IAM_ALLOWED_PRINCIPALS` retains the `ALL` permission on a table, **Lake Formation security is completely bypassed** (see [[aws-lake-formation]]). Access is determined solely by coarse S3 bucket policies and IAM roles, rendering granular column/row masks and LF-Tags completely ineffective (see [[aws-lake-formation]], [[lake-formation-security-model]]).
- **The Safe Migration Workflow**:
  1. **Register S3 Locations**: Register all data lake S3 bucket paths in Lake Formation (see [[aws-lake-formation]]).
  2. **Establish Lake Formation Grants**: Grant explicit `SELECT` permissions to the target IAM roles and users (via direct grants or LF-Tags) (see [[lake-formation-security-model]]).
  3. **Revoke Default Grants**: Revoke `ALL` permissions from `IAM_ALLOWED_PRINCIPALS` on the database and tables (see [[aws-lake-formation]]).
  4. **Enforce Default Settings**: In Lake Formation **Data catalog settings**, uncheck:
     - *"Use only IAM access control for new databases"*
     - *"Use only IAM access control for new tables in new databases"*
  5. **Remove Direct S3 IAM Policies**: Strip `s3:GetObject` permissions from user IAM roles to enforce credential vending through Lake Formation (see [[lake-formation-security-model]]).

## Cross-Account Sharing Gotchas via AWS RAM

Sharing data lake tables with external AWS accounts without data duplication relies on AWS Resource Access Manager (RAM) (see [[aws-lake-formation]]):

1. **Producer Account Grant**:
   - The producer account grants Lake Formation `SELECT` permissions to the consumer AWS Account ID and sends a RAM share invitation (see [[aws-lake-formation]]).
2. **Accepting the RAM Share**:
   - The consumer account administrator must open **AWS RAM** and accept the resource share (automatic within an AWS Organization if auto-acceptance is enabled) (see [[aws-lake-formation]]).
3. **The Resource Link Requirement**:
   - The shared table does not automatically appear in the consumer account's query tools.
   - The consumer administrator must create a **Resource Link** in their local Glue Data Catalog pointing to the shared external table (see [[aws-lake-formation]], [[glue-data-catalog]]).
4. **Local Lake Formation Grants**:
   - Consumer account users cannot query the table until the consumer administrator grants Lake Formation `DESCRIBE` and `SELECT` on the local **Resource Link** to local IAM roles (see [[aws-lake-formation]]).

## Related

- [[aws-lake-formation]]
- [[lake-formation-security-model]]
- [[aws-lake-formation-service]]
- [[glue-data-catalog]]
- [[aws-glue-job-optimization]]
- [[query-io-optimizations]]
