---
type: entity
title: AWS Lake Formation
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-lake-formation]]"]
tags: [aws, lake-formation, entity, security, governance, data-lake]
status: stable
---

# AWS Lake Formation

AWS Lake Formation is a fully managed data governance, security, and catalog authorization service provided by Amazon Web Services (see [[aws-lake-formation]]). It simplifies the creation, management, and securing of multi-tenant cloud data lakes on Amazon S3 by providing a centralized policy plane that enforces fine-grained access control across disparate analytical engines (see [[aws-lake-formation]], [[glue-data-catalog]]).

## Core Capabilities and Functional Role

Lake Formation operates as the security and governance layer of the AWS analytics stack (see [[aws-lake-formation]]):

| Capability | Functional Role | Implementation Mechanism |
| :--- | :--- | :--- |
| **Centralized Security Layer** | Replaces coarse S3/IAM bucket policies with fine-grained controls (see [[aws-lake-formation]]) | Layered over [[glue-data-catalog|AWS Glue Data Catalog]] metadata |
| **Data Filtering** | Restricts data views by column, row, or cell without physical data replication (see [[lake-formation-security-model]]) | Dynamic row filter SQL predicates and column masks |
| **Tag-Based Access Control** | Scales permissions via hierarchical key-value policy tags (**LF-Tags**) (see [[lake-formation-security-model]]) | Cascading tag inheritance (Database $\rightarrow$ Table $\rightarrow$ Column) |
| **Credential Vending** | Shields raw S3 bucket endpoints from end users (see [[lake-formation-security-model]]) | Vends short-lived STS tokens directly to query runtimes |
| **Cross-Account Sharing** | Enables zero-copy federation across AWS accounts and Organizations (see [[aws-lake-formation]]) | Integrated via AWS Resource Access Manager (RAM) |
| **Auditing & Compliance** | Tracks all catalog modifications and data access requests | Centralized integration with AWS CloudTrail (see [[aws-lake-formation]]) |

## Supported Analytical Compute Engines

Lake Formation enforces authorization policies consistently across a wide range of AWS data engines (see [[aws-lake-formation]]):
- **Serverless Querying**: Amazon Athena natively queries Lake Formation governed tables with full column and row filter enforcement (see [[aws-lake-formation]]).
- **Data Warehousing**: Amazon Redshift Spectrum respects Lake Formation grants when querying external lakehouse tables (see [[aws-lake-formation]]).
- **Distributed Big Data**: Amazon EMR (with Spark, Hive, Presto, and Trino) integrates via the Lake Formation EMR runtime authorization agent (see [[aws-lake-formation]]).
- **Serverless ETL**: AWS Glue jobs retrieve scoped credentials to read only authorized partitions and columns (see [[aws-glue-platform]], [[aws-lake-formation]]).
- **Business Intelligence**: Amazon QuickSight supports direct governed data connections (see [[aws-lake-formation]]).

## Support for Open Table Formats

Lake Formation provides native support for open table formats, including **Apache Iceberg** (see [[aws-lake-formation]], [[apache-iceberg-format]]):
- Manages transactional tables stored on S3 with ACID consistency while applying row-level and column-level security filters (see [[aws-lake-formation]]).
- Coordinates with the Glue Data Catalog to secure Iceberg metadata pointers and underlying data files (see [[iceberg-metadata-architecture]], [[apache-iceberg-format]]).

## Pricing Model

AWS Lake Formation incurs **no direct service fee or hourly subscription charge** (see [[aws-lake-formation]]):
- Users pay standard rates for the underlying services consumed during operation:
  - **Storage**: Amazon S3 standard storage fees for raw data, manifests, and tables (see [[parquet-file-architecture]]).
  - **Metadata & ETL**: AWS Glue Data Catalog object storage, API requests, and ETL DPU-hours (see [[aws-glue-architecture-and-dpu]]).
  - **Compute & Analytics**: Amazon Athena scanned data fees, Redshift cluster hours, or EMR instance hours (see [[aws-lake-formation]]).

## Related

- [[aws-lake-formation]]
- [[lake-formation-security-model]]
- [[lake-formation-access-denied-and-migration]]
- [[glue-data-catalog]]
- [[aws-glue-platform]]
- [[apache-iceberg-format]]
