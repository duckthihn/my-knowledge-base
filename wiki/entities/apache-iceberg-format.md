---
type: entity
title: Apache Iceberg Table Format
created: 2026-11-13
updated: 2026-11-13
sources: ["[[apache-iceberg]]", "[[aws-glue]]", "[[aws-athena]]"]
tags: [iceberg, entity, table-format, open-source, data-lake, ecosystem]
status: stable
---

# Apache Iceberg Table Format

Apache Iceberg is a high-performance open table format designed for massive analytical datasets stored in cloud object storage (see [[apache-iceberg]]). Created originally at Netflix to solve scalability, correctness, and performance limitations of Hive-style metastores on AWS S3, Iceberg graduated to a top-level Apache Software Foundation project and serves as a vendor-neutral standard across modern lakehouse architectures (see [[apache-iceberg]]).

## Disambiguating the Storage Stack

To avoid architectural confusion, modern lakehouse architectures separate three distinct layers (see [[apache-iceberg]]):

| Layer | Responsibility | Examples |
| :--- | :--- | :--- |
| **File Format** | Physical byte layout and compression within a single file | Apache Parquet, ORC, Avro (see [[apache-parquet-format]], [[parquet-file-architecture]]) |
| **Table Format** | Logical organization of multiple files into a transactional table with metadata | **Apache Iceberg**, Delta Lake, Apache Hudi (see [[iceberg-metadata-architecture]]) |
| **Catalog** | Centralized pointer registry storing the current metadata location of each table | [[glue-data-catalog|AWS Glue Data Catalog]], Project Nessie, Hive Metastore, REST Catalogs (see [[aws-glue]]) |

## Multi-Engine Interoperability

A core design strength of Iceberg is its strict engine neutrality (see [[apache-iceberg]]):
- **Batch & Interactive Compute**: Apache Spark, Trino, Presto, DuckDB, StarRocks (see [[apache-spark]], [[spark-execution-model]]).
- **Streaming Ingestion**: Apache Flink, Spark Structured Streaming.
- **Managed Cloud Warehouses**: Snowflake, Google Cloud BigQuery, [[aws-athena-service|Amazon Athena]], Amazon EMR, Amazon Redshift, and AWS S3 Tables (see [[apache-iceberg]], [[aws-athena]]).

Because Iceberg tables maintain self-contained metadata, **AWS Glue Crawlers and Athena Partition Projection are completely unnecessary**; compute engines discover schemas, partitions, and file locations directly from Iceberg metadata while using Glue Data Catalog strictly for atomic commit pointers (see [[apache-iceberg]], [[aws-glue]], [[glue-data-catalog]], [[athena-partition-projection]]).

## Configuring Iceberg on AWS Glue & PySpark

In AWS Glue ETL or Amazon EMR jobs, Iceberg is integrated using the `--datalake-formats iceberg` job parameter and configuring the Spark SQL catalog (see [[apache-iceberg]]):

```python
from pyspark.sql import SparkSession

spark = (SparkSession.builder
    .config("spark.sql.extensions",
            "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions")
    .config("spark.sql.catalog.glue_catalog", "org.apache.iceberg.spark.SparkCatalog")
    .config("spark.sql.catalog.glue_catalog.catalog-impl",
            "org.apache.iceberg.aws.glue.GlueCatalog")
    .config("spark.sql.catalog.glue_catalog.warehouse", "s3://my-lakehouse-bucket/warehouse/")
    .getOrCreate())
```

Once configured, PySpark executes standard DDL, DML, and time travel operations (see [[apache-iceberg]]):
```sql
-- Create an Iceberg table with hidden partitioning
CREATE TABLE glue_catalog.sales_db.orders (
    order_id bigint, country string, amount double, order_ts timestamp
) USING iceberg
PARTITIONED BY (days(order_ts))
TBLPROPERTIES ('format-version' = '2');

-- Query historical snapshot via time travel
SELECT * FROM glue_catalog.sales_db.orders TIMESTAMP AS OF '2026-11-01 00:00:00';
```

## Comparison with Alternative Lakehouse Formats

Open table formats share similar foundational goals (ACID transactions, time travel, schema evolution), but differ in design focus and ecosystem governance (see [[apache-iceberg]]):

| Feature / Trait | Apache Iceberg | Delta Lake | Apache Hudi | Hive-Style Lake |
| :--- | :--- | :--- | :--- | :--- |
| **ACID Transactions** | Yes (Snapshot isolation via OCC) | Yes (ACID log) | Yes (Timeline log) | No (Partial writes visible) |
| **Time Travel** | Yes | Yes | Yes | No |
| **Engine Independence** | Exceptional (Broad vendor backing) | High (Optimized for Databricks) | Good | High (Legacy standard) |
| **Partition Evolution** | Native (Zero data rewrites) | Limited | Limited | Impossible (Requires table rewrite) |
| **Primary Strength** | Open standard, multi-engine flexibility | Native Databricks integration | Near-real-time streaming & CDC | Simplicity |

### Format Convergence
Industry formats are rapidly converging: Databricks encodes Format v3 deletion vectors in Iceberg using the identical binary specification as Delta Lake, enabling bidirectional interoperability between Iceberg and Delta tables without data copying (see [[apache-iceberg]]).

## Related

- [[apache-iceberg]]
- [[iceberg-metadata-architecture]]
- [[iceberg-table-optimizations]]
- [[iceberg-table-maintenance]]
- [[aws-athena]]
- [[aws-athena-service]]
- [[aws-glue]]
- [[aws-glue-platform]]
- [[glue-data-catalog]]
- [[apache-parquet-format]]
- [[apache-spark]]
- [[spark-execution-model]]
