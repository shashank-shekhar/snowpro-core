# Snowflake SnowPro Core Notes

## Layers

1. **Database Storage**
2. **Query Processing (Virtual Warehouses - VW)**
3. **Everything Else**

## Cloud Platforms

* AWS
* GCP
* Azure

## Regions

* North & South America
* Europe / Africa / Middle East
* APAC & China *(GCP not available)*

> **Note:** Account is restricted to a region.

## Editions

* **Standard**
* **Enterprise**
* **Business Critical** (PHI, HIPAA)
* **Virtual Private Snowflake** *(Does not share hardware)*

## Clients

* **Snowsight** – UI Client
* **SnowSQL** – CLI Client

## Connectors

* **Snowflake Connectors** (3rd-party integrations)

## Snowpark

* Languages: Python, Java, Scala
* Code execution in **Virtual Warehouses**
* Supports **User-Defined Functions (UDFs)**

## SnowCD (Connectivity Diagnostics)

Run diagnostics:

```sql
SELECT SYSTEM$ALLOWLIST();
SELECT SYSTEM$ALLOWLIST_PRIVATE_LINK();
```

* Install and run SnowCD against the values returned.

## Streamlit

* Apps managed by **RBAC**
* Runs on **Virtual Warehouse**
* Supports Snowpark, UDF, Stored Procedures, Native frameworks

> **Note:** App & queries must run on the same VW; VW kept alive \~15 min after last connection.

## Cortex

* Built-in LLM, ML functions, Vector Search
* Managed by Snowflake

## SQL API

* REST API for SQL interaction
* Send SQL as `POST`, receive `[queryId]` and status

## SQL Databases

* Fully managed logical containers
* Features: Time Travel (up to 90 days), Fail-safe, Cross-DB queries, Zero-copy cloning, Secure data sharing

## Stages

* **User Stage**: `@~`
* **Table Stage**: `@%`
* **Named Stage**: `@CUSTOM_NAME`

## Schemas

* Permanent (default)
* Transient (cheaper, no fail-safe)
* Shared Schema
* Information Schema

## Tables

| Type      | Lifetime      | Time Travel (days) | Fail-safe |
| --------- | ------------- | ------------------ | --------- |
| Permanent | Until dropped | 0–90               | 7 days    |
| Temporary | Session       | 0–1                | 0         |
| Transient | Until dropped | 0–1                | 0         |

## Views

### Types

* Regular Views
* Secure Views (Creation query hidden, can be materialized)
* Materialized Views (Enterprise+)

### When to Use

* Fewer rows/columns than base table
* Significant processing needed
* External tables
* Infrequently changing base table

## UDF (User-Defined Functions)

* **Scalar Functions**: Java, Python, Scala, SQL, JavaScript
* Callable only from SQL

## UDTF (User-Defined Table Functions)

* Return data as tables

## Stored Procedures

* Supported languages: Java, Python, Scala, SQL, JavaScript
* Execution Modes: Caller’s rights, Owner’s rights (`EXECUTE AS OWNER`)

## Streams

* Records DML changes
* Change tables created for querying
* Available on multiple table types

## Tasks

* Scheduled calls to stored procedures
* Serverless (scheduled/triggered), Managed (concurrent/unpredictable loads)
* Scheduling via intervals or CRON
* Can form Task Graphs (`CREATE TASK T2 AFTER T1`)

## Pipes

* Automatic data loading from stages
* Triggered by cloud messaging or REST
* Snowpipe (serverless) vs Bulk Loading (VW)

## Shares

* Requires `ACCOUNTADMIN` or `CREATE SHARE`

## Sequence

* Generates row IDs similar to database sequences

## Micro Partitions

* Columnar storage, compressed (50–500 MB)
* Metadata includes value ranges, optimization info

## Clustering Keys

* Optimizes micro-partitions
* Best for large tables (>1TB) and selective queries
* Limit to 3–4 columns

## Query History & Caching

* 14-day history UI, 365-day via `QUERY_HISTORY`
* Result Cache valid for 24 hours

## Resource Monitor

* Requires `ACCOUNTADMIN`
* Monitors usage, can suspend resources

## Access Control

* **DAC**: Owner-granted access
* **RBAC**: Privileges assigned via roles

## Availability Zone & Snowgrid

* Data replication within zones
* Snowgrid connects regions and clouds

## Data Loading

* Small uploads via **Snowsight**
* Larger uploads via `PUT` and `COPY INTO` (100–250 MB ideal)
* **Snowpipe** incurs ingestion charges

## Time Travel & Fail-safe

* **Time Travel**: 1–90 days
* **Fail-safe**: 7 days (disaster recovery)

## Zero Copy Cloning

* Instant, storage-efficient cloning

## Encryption

* Snowflake-managed, customer-managed, or Tri-Secret Secure
* Keys rotate every 30 days

## Unloading

* Formats: CSV, JSON, Parquet

## Data Sharing

* Secure views/materialized views shareable
* Metadata-based sharing (no storage cost, compute-only)
* Reader accounts available

## Search Optimization

* Maintains search paths
* Best for non-clustered columns, complex queries

## Best Practices

* Avoid using `ACCOUNTADMIN` for automated scripts
* Minimum privileges: `USAGE` on DB/schema, `SELECT` on table

## Future Grants

* Grants privileges automatically to future objects
* Managed Access Schema restricts sharing
  
## Numbers to Remember

| Value     | Description                     |
| --------- | ------------------------------- |
| 365       | COPY\_HISTORY (ACCOUNT\_USAGE)  |
| 14        | UI table copy history           |
| 30        | SF-managed key rotation         |
| 7         | PIPE staged file window (days)  |
| 50–500 MB | Micro-partition size            |
| 1 TB+     | Ideal clustering key table size |
| 16 MB     | Default unloading size (`COPY`) |
| 64 days   | Bulk load metadata retention    |
| 128 MB    | Max VARIANT value size          |

---
