---
type: entity
title: AWS Glue Serverless Data Integration
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-glue]]", "[[aws-lake-formation]]"]
tags: [aws, glue, entity, serverless, etl, data-lake, spark]
status: stable
---

# AWS Glue Serverless Data Integration

AWS Glue is a fully managed, serverless data integration and event-driven ETL (Extract, Transform, Load) service provided by Amazon Web Services (see [[aws-glue]]). It abstracts the underlying compute infrastructure, enabling data engineers to author, schedule, and monitor distributed Apache Spark, Python, and Ray data pipelines without provisioning or managing server clusters (see [[aws-glue]], [[apache-spark]]).

## Platform Architecture and Sub-Services

AWS Glue functions as an ecosystem composed of several integrated services (see [[aws-glue]]):

| Service Component | Operational Role | Analogy / Equivalent |
| :--- | :--- | :--- |
| **Data Catalog** | Centralized metadata repository storing schemas, tables, and S3 partition locations (see [[glue-data-catalog]]) | Apache Hive Metastore (see [[aws-glue]]) |
| **ETL Jobs** | Distributed compute engines running PySpark (`glueetl`), Streaming, Python Shell, and Ray (see [[aws-glue-architecture-and-dpu]]) | Ephemeral Spark applications (see [[learn-spark]]) |
| **Crawlers** | Automated bots scanning data stores to infer schemas and register metadata into the Catalog (see [[aws-glue]]) | Metadata discovery robot |
| **Workflows & Triggers** | Lightweight orchestrator coordinating dependent chains of jobs and crawlers via event, cron, or conditional triggers (see [[aws-glue]]) | Lightweight DAG orchestrator |
| **Interactive Sessions** | Serverless Jupyter notebook kernels connecting directly to Glue Spark runtimes for rapid cell-by-cell debugging (see [[aws-glue]]) | Remote interactive PySpark REPL |
| **Data Quality** | Rule-based engine defining and evaluating dataset validation constraints (e.g., null checks, completeness) (see [[aws-glue]]) | Great Expectations |
| **Schema Registry** | Centralized schema contract management for streaming pipelines (Kafka, Amazon Kinesis) preventing schema drift | Confluent Schema Registry |

## Serverless Compute Model

Unlike traditional on-premise clusters or Amazon EMR, Glue decouples infrastructure administration from pipeline code (see [[aws-glue]], [[apache-spark]]):
- Users specify transformation logic and compute capacity in **Data Processing Units (DPUs)** (1 DPU = 4 vCPUs and 16 GB RAM) (see [[aws-glue-architecture-and-dpu]]).
- AWS automatically handles cluster provisioning, software installation, driver/executor placement, and resource deallocation upon completion (see [[aws-glue]]).
- Jobs support **Auto-Scaling** and **Flex Execution** modes to balance elasticity with cost efficiency (see [[aws-glue-architecture-and-dpu]], [[aws-glue-job-optimization]]).

## AWS Ecosystem Integration

AWS Glue serves as the foundational data lakehouse integration layer on AWS (see [[aws-glue]]):
- **Object Storage & Analytics**: Natively reads and writes Parquet, ORC, Avro, JSON, and CSV on **Amazon S3** (see [[parquet-file-architecture]]).
- **Serverless Querying**: Tables defined in the Glue Data Catalog are instantly queryable in **Amazon Athena** and **Amazon Redshift Spectrum** without data movement (see [[glue-data-catalog]]).
- **Security & Governance**: Coordinates with **AWS Lake Formation** to enforce column-level masks, row-level filters, and LF-Tag based access control over catalog tables (see [[aws-lake-formation-service]], [[lake-formation-security-model]]).
- **Lakehouse Table Formats**: Native support for **Apache Iceberg**, Delta Lake, and Apache Hudi via the `--datalake-formats` configuration, enabling transactional ACID updates and time travel over S3 data lakes (see [[apache-iceberg-format]], [[aws-glue]]).

## Related

- [[aws-glue]]
- [[aws-glue-architecture-and-dpu]]
- [[dynamicframe-vs-dataframe]]
- [[glue-data-catalog]]
- [[aws-glue-job-optimization]]
- [[aws-lake-formation]]
- [[aws-lake-formation-service]]
- [[lake-formation-security-model]]
- [[apache-iceberg-format]]
- [[apache-spark]]
- [[parquet-file-architecture]]
