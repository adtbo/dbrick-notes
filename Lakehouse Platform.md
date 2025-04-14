**A lakehouse has the following key features:**
- **Transaction support:** In an enterprise lakehouse many data pipelines will often be reading and writing data concurrently. Support for ACID transactions ensures consistency as multiple parties concurrently read or write data, typically using SQL.
- **Schema enforcement and governance:** The Lakehouse should have a way to support schema enforcement and evolution, supporting DW schema architectures such as star/snowflake-schemas. The system should be able to [reason about data integrity](https://www.databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html), and it should have robust governance and auditing mechanisms.
- **BI support:** Lakehouses enable using BI tools directly on the source data. This reduces staleness and improves recency, reduces latency, and lowers the cost of having to operationalize two copies of the data in both a data lake and a warehouse.
- **Storage is decoupled from compute:** In practice this means storage and compute use separate clusters, thus these systems are able to scale to many more concurrent users and larger data sizes. Some modern data warehouses also have this property.
- **Openness:** The storage formats they use are open and standardized, such as Parquet, and they provide an API so a variety of tools and engines, including machine learning and Python/R libraries, can efficiently access the data **directly**.
- **Support for diverse data types ranging from unstructured to structured data**: The lakehouse can be used to store, refine, analyze, and access data types needed for many new data applications, including images, video, audio, semi-structured data, and text.
- **Support for diverse workloads:** including data science, machine learning, and SQL and analytics. Multiple tools might be needed to support all these workloads but they all rely on the same data repository.
- **End-to-end streaming:** Real-time reports are the norm in many enterprises. Support for streaming eliminates the need for separate systems dedicated to serving real-time data applications.

![[data-control2.png]]

![[data-control.png]]

Delta tables stored in a Directory where parquet data files are stored, a sub directory _delta_log where metadata, and the transaction log is stored as JSON files

Hint :
- Cold start => Serverless
- Reduce cost => Auto stop
- Throughput / Sequential => Scale Up / Cluster Size
- Performance / Concurrent => Scale Out / Scaling

default location for `CREATE DATABASE sample_db` is `dbfs:/user/hive/warehouse`

Repo is now GIT Folder. repo can't do merge.

### Repo (Legacy)
![[repos.png]]

### Git Folder
![[git.png]]

| Aspect         | Managed Table                                | External Table                                 |
| -------------- | -------------------------------------------- | ---------------------------------------------- |
| Data Ownership | System manages both metadata & data          | User manages the data, system handles metadata |
| Data Deletion  | Deleting the table deletes the data          | Deleting the tables leaves the data intact     |
| Best for       | Internal datasets or temporary tables        | Pre-existing datasets or shared data           |
| Flexibility    | Less flexible, tightly coupled to the system | More flexible, decouple from the system        |

### **Comparison of Databricks View Types**

|**Feature**|**Standard View**|**Materialized View**|**Temporary View**|**Dynamic View**|**Hive Metastore View (Legacy)**|
|---|---|---|---|---|---|
|**Definition**|Read-only query result over tables/views|Pre-computed, incrementally updated table|Short-lived, session-scoped view|View with row/column-level access control|Legacy Hive-compatible view|
|**Storage**|Only query text (no data processed)|Results stored as Delta table|No persistent storage|Query text only|Query text only|
|**Scope**|Catalog.schema.view (Unity Catalog)|Catalog.schema.view (Unity Catalog)|Notebook/job (Notebooks) or query (SQL)|Catalog.schema.view (Unity Catalog)|Hive metastore|
|**Persistence**|Persistent until deleted|Persistent, auto-refreshed|Dropped after session ends|Persistent until deleted|Persistent until deleted|
|**Performance**|Recomputes on each query|Faster (pre-computed results)|Recomputes on each query|Recomputes on each query|Recomputes on each query|
|**Use Cases**|Logical data abstraction|Frequent queries on large datasets|Temporary analysis|Data security/masking|Legacy compatibility|
|**Governance**|Unity Catalog permissions|Unity Catalog permissions|No permissions (session-only)|Unity Catalog + RBAC|Hive metastore permissions|
|**Recommended Usage**|✅ Preferred for most read-only queries|✅ For performance-critical queries|⚠️ Temporary work|✅ Row/column-level security|❌ Migrate to Unity Catalog|