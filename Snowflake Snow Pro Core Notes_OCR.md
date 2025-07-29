# Layers

1. **Database Storage**
2. **Query Processing** → VWH (Virtual Warehouses)
3. **Everything Else**

---

# Cloud Platforms

- AWS  
- GCP  
- Azure

---

# Regions

- North & South America  
- Europe / Africa / Middle East  
- APAC & China *(GCP not available)*

---

> **Note**:  
> Account is restricted to a region.


Here is the properly formatted response in **Markdown**:

---

## Editions

- **Standard**
- **Enterprise**
- **Business Critical** (PHI, HIPAA)
- **Virtual Private Snowflake**
  - Does not share hardware (H/W)

---

## Clients

- **Snowsight** – UI Client
- **SnowSQL** – CLI Client

---

## Connectors

- **Snowflake Connectors**
  - 3rd-party integrations

---

## Snowpark

- Write code in:
  - Python
  - Java
  - Scala
- Code can run in **Virtual Warehouses (VW)**
- Can create **User-Defined Functions (UDFs)**

---

## SnowCD (Connectivity Diagnostics)

- Run diagnostics:
  ```sql
  SELECT SYSTEM$ALLOWLIST();
  SELECT SYSTEM$ALLOWLIST_PRIVATE_LINK();
```

- Install and run SnowCD against the values returned above
    

---

## Streamlit

- Apps object controlled by **RBAC**
    
- Runs on **Virtual Warehouse**
    
- Supports:
    
    - Snowpark
    - UDF
    - Stored Procedures (SP)
    - Native app frameworks
        

> **Note**:
> - App & queries must run on the same VW
> - VW kept alive approx. 15 min after last socket connection

---

## Cortex

- **Built-in features**:
    
    - Built-in LLM
    - Pre-built ML functions
    - Vector Search
- **Hosting**: Managed by Snowflake
    

---

## SQL API

- REST API to interact with data
- Send SQL as `POST` payload
- Receive `[queryId]` and status

---

## SQL Databases

- Fully managed logical containers
    
- Features:
    
    - **Time Travel** (up to 90 days)
    - **Fail-safe**
    - **Cross-DB queries**
    - **Zero-copy cloning**
    - **Secure data sharing**

---

## Stages

- **User Stage**: `@~`
- **Table Stage**: `@%`
- **Named Stage**: `@YOU_CUSTOM_NAME`

---

## Schemas

- **Permanent** – Default
- **Transient** – Cheaper, no fail-safe
- **Shared Schema**
- **Information Schema**

---

## Tables

|Type|Lifetime|Time Travel|Fail-safe|
|---|---|---|---|
|Permanent|Until dropped|0–1 (Standard), 0–90 (Rest)|7 days|
|Temporary|Session|0–1|0|
|Transient|Until dropped|0–1|0 (invoked by support)|

---
# Views

## Types

- **Regular Views**
- **Secure Views**
  - Creation query not visible
  - Can be materialized

- **Materialized Views**
  - Available in Enterprise edition and higher
  - Persists output of `SELECT`

## When to Use

- Fewer rows/columns compared to base table
- Query requires significant processing
- On external tables
- Base table doesn't change often

---

# UDF (User-Defined Functions)

## Scalar Functions

- One output row per input row
- Supported Languages:
  - Java
  - Python
  - Scala
  - SQL
  - JavaScript
- Only callable from SQL

---

# UDTF (User-Defined Table Functions)

- Same as UDFs, but return data as a **table**

---

# Stored Procedures

- Languages Supported:
  - Java
  - Python
  - Scala
  - SQL
  - JavaScript

## Execution Modes

- **Caller’s rights stored proc**
- **Owner’s rights stored proc**
  - Use `EXECUTE AS OWNER`

---

# Streams

- Records DML changes to tables
- Creates a **Change Table** that can be queried

## Also Available On

- Directory tables
- Dynamic tables
- Iceberg tables
- Event tables
- External tables

> A table version is created with every DML operation

---

# Tasks

- Can run **Serverless** or **Managed** in a warehouse
- All language support

## Recommendations

- Serverless:
  - Best for scheduled or triggered tasks
- Managed:
  - Best for concurrent tasks or unpredictable loads

## Scheduling

- Use intervals like `'10 MINUTES'` or `CRON`
- Supports **Fail or Retry on failure**

---

# Pipes

## Overview

- Automatic data loading when available in a **stage**
- Triggered via:
  - Cloud messaging
  - REST endpoints

## Comparison

| Feature             | Snowpipe                  | Bulk Loading            |
|---------------------|---------------------------|--------------------------|
| Trigger             | Cloud messaging / REST    | Manual or external      |
| Auth                | JWT                       | User's context          |
| Metadata Retention  | 14-day metadata           | 64-day metadata         |
| Transactions        | Multiple                  | Single transaction      |
| Execution           | Serverless                | Runs on Virtual Warehouse (VW) |
# Shares

- Requires `ACCOUNTADMIN` or `CREATE SHARE` privilege

---

# Sequence

- Similar to a database sequence generator
- Used for generating row IDs

---

# Micro Partitions

- Size: 50–500 MB uncompressed
- Always stored compressed
- Metadata includes:
  - Range of values
  - Number of distinct values
  - Optimization information
- Stored in columnar storage

### Benefits

- Faster table drop (metadata only)
- Faster column deletion
- Query pruning

---

# Clustering Keys

- Defining clustering improves micro-partition organization
- Especially useful for **materialized tables**

### Ideal Candidates

- Tables larger than 1 TB
- Tables with highly selective queries

### Strategy

- Use a maximum of 3–4 columns

---

# Query History & Caching

## Query History

- 14-day history available in UI
- 365-day history via `QUERY_HISTORY`
- Tracked by:
  - Query text
  - Execution status
  - Micro-partitions scanned
  - Timing & bytes scanned (slow query indicator)
  - Spillages
  - % of data scanned from cache

## Query Result Cache

- Valid for 24 hours
- Case-sensitive match on query text
- Cleared when the Virtual Warehouse (VW) is suspended

---

# Context

### Key Aspects

- **Security** – Role management
- **Performance** – Size of VW
- **Error prevention** – Correct setup

---

# Resource Monitor

- Requires `ACCOUNTADMIN` privilege
- Can **notify** and/or **suspend** resources
- Default frequency: **Monthly**

### Hierarchy

```text
      Resource Monitor
     ↙        ↓        ↘
   VW1      VW2      VW3
```

- Individual Resource Monitor can override account-level settings
- **Reset** occurs on billing cycle or quota/threshold increase
- Monitors can be dropped when no longer needed

# Access Control

## DAC - Discretionary Access Control

- Each object has an owner who can grant access to it.

## RBAC - Role-Based Access Control

- Privileges are assigned to **roles**, not directly to users.

---

# Availability Zone

- Data replication occurs within a zone.

---

# Snowgrid

- Connects regions and cloud providers.

---

# Data Loading

- **Snowsight** – for small data uploads
- **SQL / SnowSQL**:
  - `PUT` – upload file to stage
  - `COPY INTO` – load from stage to table
  - Ideal file size: 100–250 MB
- **Snowpipe** – has ingestion charges

### Data Flow

```text
Stage → Pipe → Table
```

- All objects belong to:
    - A **schema** → belongs to a **database** → belongs to an **account**
---

# Time Travel & Fail-safe

- **Time Travel**: 1–90 days (to recover historical data)
- **Fail-safe**: 7 days, for disaster recovery (only accessible by Snowflake support)

---

# Zero Copy Cloning

- Instant copy
- No additional storage unless data is changed in the clone

---

# Access Roles Overview

## i) ACCOUNTADMIN

- Top-level role
- Can manage billing and credits, and stop running SQL
- Use MFA, and restrict number of users in this role (recommend 2+)

## ii) USERADMIN

- Child of `SECURITYADMIN`
- Can create/manage users and roles

## iii) SECURITYADMIN

- Can `GRANT` or `REVOKE` privileges on accounts

## iv) SYSADMIN

- Can create virtual warehouses, databases, and all database objects

---

# Best Practices

- Avoid using `ACCOUNTADMIN` for automated scripts

## Minimum Privileges to Query a Table

- Database: `USAGE`
- Schema: `USAGE`
- Table: `SELECT`
    

---

# Future Grants

- Grant privileges to all future objects of a type in a database/schema

## Managed Access Schema

- Restricts object sharing to roles with `GRANT`, `ACCESS`, or `MANAGE GRANT` privileges
- Only object owner or schema owner can grant access

---

# With Managed Access

- Transfers ability to share objects from object owner to schema owner

---

## Access Control Models

- **DAC** – Discretionary Access Control
- **RBAC** – Role-Based Access Control

---

## Securable Objects

- Databases, Users, Roles, Virtual Warehouses

### Public Role

- Granted to every user by default
- Any object created by this role is available to all users
---
# Encryption

- Use **Snowflake managed keys** or **customer managed keys**
- **Tri-Secret Secure**: combines both
- Keys rotate every **30 days**
  - Old data is decrypted using the old key
- **Rekeying** (Enterprise only): 
  - Destroys old keys and re-encrypts with new keys

---

# Data Loading (Snowpipe)

- Loads data from **stage** into **table**
- **Does NOT** delete staged data
- `ALTER PIPE REFRESH`: auto-ingests last 7 days of staged files

---

# Unloading

- Supported formats: `csv`, `json`, `parquet`

---

# Data Sharing

- Requires `CREATE SHARE`
- Share **most database and child objects**
- Only **Secure Views** and **Secure Materialized Views** can be shared
- Shared data uses **metadata** only (no storage cost)
- Cost is for **compute** only
- Share is created **between accounts**
- **Reader accounts** do not need a full Snowflake account

---

# Micro Partitions

- Data is stored in a **columnar** format
- Query results can be persisted to survive **VW suspension**
- Supports:
  - `UNDROP`
  - **Streams**
  - **Clustered tables**
  - **Point lookup optimization**
  - **Tokenization**
  - **Lateral Flatten**
  - **Search optimization**
  - **Tasks**

---

# Numbers

| Value      | Description                                         |
|------------|-----------------------------------------------------|
| 365        | `COPY_HISTORY` in `ACCOUNT_USAGE`                  |
| 14         | UI table copy history                               |
| 30         | SF managed key rotation                             |
| 7          | PIPE refresh staged file consumption window (days) |
| 14         | Pipe pause duration without being stale             |
| 50–500 MB  | Uncompressed micro-partition size                   |
| 1 TB+      | Clustering key table size (ideal)                   |
| 16 MB      | Default unloading file size for `COPY`              |
| 64 days    | Bulk load history in table metadata                 |
| 128 MB     | Max `VARIANT` value size                            |

---
# Tasks

- A **Task** is a scheduled call to a stored procedure
- Can be **serverless** for:
  - Scheduled execution
  - Data arrival-based execution

---

# Streams

- Used to **perform change data capture (CDC)**

---

# Task Graph

```sql
CREATE TASK T1;
CREATE TASK T2 AFTER T1;
```

- Create a **finalize** task as the root if needed

---
# Search Optimization (Search)

- Maintains **search access path**
- Tracks which micro-partitions contain a value

## Suitable For

- **Non-clustered columns**
- Queries that:
    - Take a few seconds or more
    - Target columns with **100K+ distinct values**

---