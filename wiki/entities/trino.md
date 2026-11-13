---
type: entity
title: Trino (PrestoSQL)
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-athena]]"]
tags: [trino, presto, entity, query-engine, mpp, sql, open-source, analytics]
status: stable
---

# Trino (PrestoSQL)

**Trino** (formerly **PrestoSQL**, forked from Facebook's Presto in 2020) is an open-source, distributed Massively Parallel Processing (MPP) SQL query engine designed for fast, interactive analytics directly over data stored in heterogeneous sources — including cloud object storage (S3, GCS), relational databases, NoSQL stores, and data lakehouses (see [[aws-athena]]).

## Architecture

Trino uses a **coordinator–worker** architecture where all query processing is fully in-memory across a worker fleet (see [[aws-athena-architecture-and-cost]]):

```
Client SQL Query
   ↓
[Coordinator]
   • Parses and plans the query
   • Fetches metadata from catalogs (Hive Metastore, Glue, Iceberg REST)
   • Decomposes execution into distributed splits
   • Collects and returns results
   ↓
[Worker Fleet]
   • Each worker reads assigned data splits from storage in parallel
   • Performs in-memory filtering, hashing, joins, aggregations
   • Exchanges intermediate results via in-memory shuffle (no disk spill by default)
```

Key distinction from Apache Spark: Trino is **purpose-built for interactive SQL** (sub-second to minutes), not general distributed computing. It has no equivalent to Spark's RDD/DataFrame API or ML libraries (see [[spark-execution-model]]).

## Trino vs. Apache Spark

| Dimension | Trino | Apache Spark |
|---|---|---|
| **Primary Use Case** | Interactive SQL analytics | General distributed data processing |
| **Execution Model** | Pipelined, fully in-memory MPP | DAG of stages with disk-spill tolerance |
| **Fault Tolerance** | Query restarts on failure (no partial recovery) | Stage-level retry via RDD lineage |
| **API Surface** | SQL only | SQL + DataFrame + Streaming + ML |
| **Latency Profile** | Sub-second to minutes | Minutes to hours (batch) |
| **Managed AWS Service** | Amazon Athena (see [[aws-athena-service]]) | AWS Glue, Amazon EMR (see [[aws-glue-platform]]) |

## Role in the AWS Lakehouse Stack

Amazon Athena is the primary managed Trino service on AWS, providing serverless access to Trino without cluster provisioning (see [[aws-athena-service]], [[lakehouse-architecture]]):

- Trino queries metadata from the [[glue-data-catalog|AWS Glue Data Catalog]] for schema and partition information.
- Trino workers read Parquet, ORC, Avro, and JSON data directly from Amazon S3 (see [[parquet-file-architecture]]).
- Trino natively reads **Apache Iceberg** metadata manifests for file pruning and time travel (see [[iceberg-metadata-architecture]], [[apache-iceberg-format]]).
- Access control is enforced by **AWS Lake Formation** via credential vending (see [[lake-formation-security-model]]).

## Open Table Format Support

Trino (and by extension Athena) natively supports all three major open table formats:

| Format | Trino Capability |
|---|---|
| **Apache Iceberg** | Full read/write, time travel, partition evolution, Format v3 |
| **Delta Lake** | Read support (via Delta connector) |
| **Apache Hudi** | Read support (via Hudi connector) |

## Related

- [[aws-athena-service]]
- [[aws-athena-architecture-and-cost]]
- [[lakehouse-architecture]]
- [[apache-iceberg-format]]
- [[glue-data-catalog]]
- [[lake-formation-security-model]]
- [[parquet-file-architecture]]
- [[spark-execution-model]]
