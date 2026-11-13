---
type: concept
title: DynamicFrame vs. Spark DataFrame in AWS Glue
created: 2026-11-13
updated: 2026-11-13
sources: ["[[aws-glue]]"]
tags: [aws, glue, spark, dynamicframe, dataframe, schema-evolution, optimization]
status: stable
---

# DynamicFrame vs. Spark DataFrame in AWS Glue

AWS Glue extends Apache Spark by introducing the **DynamicFrame**, an alternative distributed data abstraction engineered specifically for schema-on-read flexibility and semi-structured, dirty datasets (see [[aws-glue]]). While the standard Spark DataFrame requires a rigid schema and leverages the Catalyst query optimizer, the DynamicFrame tracks schemas per record and resolves data typing anomalies, establishing a complementary relationship between the two structures (see [[aws-glue]]).

## Architectural Comparison

The fundamental distinctions between Spark DataFrames and Glue DynamicFrames center on schema enforcement and optimization engine integration (see [[aws-glue]]):

| Dimension | Spark DataFrame | Glue DynamicFrame |
| :--- | :--- | :--- |
| **Schema Paradigm** | Schema-on-write; rigid schema across all rows | Schema-on-read; dynamic, self-describing per record (see [[aws-glue]]) |
| **Mixed Data Types** | Incompatible; column must have a single defined type | Handled via the native `choice` type (e.g., `int` and `string`) (see [[aws-glue]]) |
| **Schema Drift Handling** | Fails or requires explicit schema evolution | Absorbs new fields and altered types automatically (see [[aws-glue]]) |
| **Catalyst Optimization** | Full optimization (whole-stage code gen, Tungsten) | Limited; operates outside standard Catalyst planning (see [[aws-glue]]) |
| **Ideal Operational Stage** | Core transformations, complex joins, aggregations | Ingestion of raw, semi-structured, or dirty data (see [[aws-glue]]) |

## Handling Dirty Data with `resolveChoice`

In production data lakes, incoming JSON or CSV records frequently contain type inconsistencies (for instance, an `order_id` appearing as an integer in some records and as a string in others) (see [[aws-glue]]):

- **In Spark DataFrame**: Reading this data forces Spark to either cast all values to strings or drop unparseable rows, often causing pipeline crashes (see [[aws-glue]]).
- **In Glue DynamicFrame**: Glue represents the column as a `choice` type containing both representations (see [[aws-glue]]). The engineer then deterministically resolves the ambiguity using `resolveChoice()`:

```python
# Cast all representations of 'order_id' to string, or branch into separate columns
cleaned_dyf = dyf.resolveChoice(
    specs=[("order_id", "cast:string"), ("amount", "cast:double")]
)
```

Available `choiceOption` resolutions include:
- `cast:<type>`: Uniformly coerces all values to a target primitive type.
- `project:<type>`: Retains only values matching the specified type, setting others to null.
- `make_cols`: Splits the choice into two distinct columns (e.g., `order_id_int` and `order_id_string`).

## The Performance Trade-Off: Catalyst vs. DynamicFrame

While DynamicFrames excel at schema resilience, they lack the extensive physical optimizations of Apache Spark (see [[aws-glue]], [[spark-execution-model]]):
- Spark DataFrames compile logical query plans into optimized physical plans using the **Catalyst Optimizer** and manage memory off-heap via the **Tungsten Engine** (see [[learn-spark]], [[spark-execution-model]]).
- DynamicFrames process records without whole-stage code generation and full predicate pushdown, making complex transformations (such as multi-way joins and window functions) considerably slower and more memory-intensive (see [[aws-glue]]).

## The Recommended Hybrid Architecture

To combine schema safety with peak execution performance, production Glue scripts should adopt a three-tier hybrid pipeline (see [[aws-glue]]):

```
Raw S3 Data 
    │
    ▼
[1. DynamicFrame Ingestion] ──> Resilient to schema drift & missing fields
    │
    ▼
[2. resolveChoice & Typing] ──> Normalize types and prune null columns
    │
    ▼ .toDF()
[3. Spark DataFrame]        ──> Execute complex filtering, joins, groupBy,
    │                           and windowing with full Catalyst optimization
    ▼ (Spark or .fromDF())
Clean Parquet Output on S3
```

### PySpark Implementation Pattern
```python
# 1. Ingest dirty raw data using DynamicFrame
dyf = glueContext.create_dynamic_frame.from_catalog(
    database="raw_db",
    table_name="incoming_events",
    transformation_ctx="read_raw"
)

# 2. Resolve ambiguous types
resolved_dyf = dyf.resolveChoice(specs=[("amount", "cast:double")])

# 3. Convert to Spark DataFrame for optimized transformation
df = resolved_dyf.toDF()
optimized_result = (df.filter("amount > 0")
                      .groupBy("category")
                      .sum("amount"))

# 4. Write output using Spark directly (or convert back to DynamicFrame if using Glue sinks)
optimized_result.write.mode("overwrite").parquet("s3://bucket/curated/revenue/")
```

## Related

- [[aws-glue]]
- [[aws-glue-platform]]
- [[aws-glue-architecture-and-dpu]]
- [[aws-glue-job-optimization]]
- [[glue-data-catalog]]
- [[spark-execution-model]]
- [[debugging-slow-spark-jobs]]
