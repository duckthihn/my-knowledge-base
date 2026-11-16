# Wiki Activity Log

## [2026-11-13] ingest | Spark Fundamentals: Day 1 Notes
- Source: raw/learn-spark.md
- Created: [[learn-spark]], [[spark-execution-model]], [[spark-partitioning-and-shuffle]], [[apache-spark]], [[debugging-slow-spark-jobs]]
- Updated: none
- Contradictions: none

## [2026-11-13] ingest | Apache Parquet
- Source: raw/apache-parquet.md
- Created: [[apache-parquet]], [[parquet-file-architecture]], [[query-io-optimizations]], [[apache-parquet-format]], [[parquet-small-files-problem]]
- Updated: [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]]
- Contradictions: none

## [2026-11-13] ingest | Apache Iceberg
- Source: raw/apache-iceberg.md
- Created: [[apache-iceberg]], [[iceberg-metadata-architecture]], [[iceberg-table-optimizations]], [[apache-iceberg-format]], [[iceberg-table-maintenance]]
- Updated: [[query-io-optimizations]], [[parquet-small-files-problem]], [[debugging-slow-spark-jobs]]
- Contradictions: none

## [2026-11-13] ingest | AWS Glue
- Source: raw/aws-glue.md
- Created: [[aws-glue]], [[aws-glue-architecture-and-dpu]], [[dynamicframe-vs-dataframe]], [[glue-data-catalog]], [[aws-glue-platform]], [[aws-glue-job-optimization]]
- Updated: [[spark-partitioning-and-shuffle]], [[debugging-slow-spark-jobs]], [[apache-iceberg-format]]
- Contradictions: none

## [2026-11-13] ingest | AWS Lake Formation
- Source: raw/aws-lake-formation.md
- Created: [[aws-lake-formation]], [[lake-formation-security-model]], [[lake-formation-access-denied-and-migration]], [[aws-lake-formation-service]]
- Updated: [[glue-data-catalog]], [[aws-glue-platform]]
- Contradictions: none

## [2026-11-13] ingest | AWS Athena
- Source: raw/aws-athena.md
- Created: [[aws-athena]], [[aws-athena-architecture-and-cost]], [[athena-partition-projection]], [[aws-athena-service]], [[athena-query-optimization-and-cost]]
- Updated: [[query-io-optimizations]], [[glue-data-catalog]], [[apache-iceberg-format]]
- Contradictions: none

## [2026-11-13] query | Cách hoạt động của Iceberg & use cases
- Sources consulted: [[apache-iceberg]], [[iceberg-metadata-architecture]], [[iceberg-table-optimizations]], [[apache-iceberg-format]], [[iceberg-table-maintenance]]
- Saved output: outputs/iceberg-how-it-works-and-use-cases.md
- Created: [[lakehouse-architecture]] (wiki/topics/)
- Updated: [[index.md]] (Topics section)
- Contradictions: none

## [2026-11-13] lint | Full vault lint check
- Issues found: 1 orphan page, 3 missing concept/entity pages
- Fixed orphan [[lakehouse-architecture]]: added inbound links from [[apache-iceberg-format]], [[glue-data-catalog]], [[query-io-optimizations]]
- Created: [[adaptive-query-execution]], [[delta-lake]], [[trino]]
- Updated: [[index.md]] (Concepts + Entities sections)
- Contradictions: none

## [2026-11-13] query | So sánh Delta Lake vs Apache Iceberg
- Sources consulted: [[delta-lake]], [[apache-iceberg-format]], [[lakehouse-architecture]], [[iceberg-metadata-architecture]], [[iceberg-table-optimizations]], [[iceberg-table-maintenance]]
- Saved output: outputs/delta-lake-vs-iceberg.md
- Updated: none
- Contradictions: none

## [2026-11-15] ingest | Automation Optimize Data Lakehouse (Iceberg, Delta Lake, Hudi)
- Source: raw/bài toán automation optimize data lakehouse (iceberg, delta lake, hudi).md
- Created: [[lakehouse-automation-optimize]], [[automated-lakehouse-optimization]]
- Updated: [[iceberg-table-maintenance]], [[lakehouse-architecture]], [[index.md]]
- Contradictions: none

## [2026-11-16] lint | Full vault lint check
- Issues found: 2 orphan pages ([[adaptive-query-execution]], [[trino]]), 4 Markdown `[text](raw/...)` links in source pages, 12 pages >800 words, 1 un-ingested raw file (`raw/bài toán crawling data.md`)
- Fixed orphans: added inbound links to [[adaptive-query-execution]] from [[spark-partitioning-and-shuffle]] and to [[trino]] from [[aws-athena]], [[aws-athena-service]], [[aws-athena-architecture-and-cost]]
- Fixed formatting: replaced Markdown `raw/` links with inline code in [[apache-iceberg]], [[aws-athena]], [[aws-glue]], [[aws-lake-formation]]
- Contradictions: none

## [2026-11-16] query | Lưu ý viết và quản lý AWS Glue Jobs (PySpark) tối ưu
- Sources consulted: [[aws-glue-job-optimization]], [[aws-glue-architecture-and-dpu]], [[dynamicframe-vs-dataframe]], [[glue-data-catalog]], [[adaptive-query-execution]], [[debugging-slow-spark-jobs]], [[parquet-small-files-problem]], [[spark-partitioning-and-shuffle]], [[spark-execution-model]], [[apache-iceberg-format]]
- Saved output: outputs/aws-glue-pyspark-best-practices.md
- Updated: none
- Contradictions: none

## [2026-11-16] query | Broadcast join là gì, hoạt động như thế nào
- Sources consulted: [[spark-partitioning-and-shuffle]], [[spark-execution-model]], [[debugging-slow-spark-jobs]], [[adaptive-query-execution]], [[learn-spark]], [[apache-spark]]
- Saved output: outputs/broadcast-join-mechanics.md
- Updated: none
- Contradictions: none

## [2026-11-16] query | Coalesce là gì và tác dụng của nó
- Sources consulted: [[spark-partitioning-and-shuffle]], [[parquet-small-files-problem]], [[adaptive-query-execution]], [[aws-glue-job-optimization]], [[debugging-slow-spark-jobs]], [[spark-execution-model]], [[learn-spark]]
- Saved output: outputs/spark-coalesce-guide.md
- Updated: none
- Contradictions: none

