`dbutils.notebook.run("full notebook path")` - command to run one notebook from another notebook.

comparison should use == and True should not be string

`DESCRIBE HISTORY table_name`

`table_identifier VERSION AS OF version`
`table_identifier TIMESTAMP AS OF timestamp_expression`

`RESTORE TABLE target_table TO VERSION AS OF <version>`
`RESTORE TABLE target_table TO TIMESTAMP AS OF <timestamp>`

https://docs.databricks.com/aws/en/delta/history

python string formatting f-strings

## define UDF
```
CREATE FUNCTION combine_nyc (city STRING)
RETURNS STRING
RETURN CASE
WHEN city = "brooklyn" THEN "new york"
ELSE city
END;
```

```
parsed_eventsDF = (events_stringsDF
	.select(from_json("value", schema_of_json(json_string)).alias("json"))
	.select("json.*")
)
```

```
SELECT orderDate, SUM(unitsSold)
FROM orderDetail od
JOIN (select orderDate, ___________(orderIds) as orderId FROM orders) o
ON o.orderId = od.orderId
GROUP BY orderDate
```

FLATTEN : [[1, 2], [3, 4]] => [1, 2, 3, 4]
EXPLODE : [1, 2, 3] =>
1
2
3

|**Operation**|**Purpose**|**Input Tables**|**Output**|**Key Requirements**|**Example**|
|---|---|---|---|---|---|
|**UNION**|Combines rows from two queries|2+ tables/queries|All distinct rows (or all rows with `UNION ALL`)|Same number of columns + compatible data types|`SELECT a FROM t1 UNION SELECT b FROM t2`|
|**INTERSECT**|Returns only common rows|2 tables/queries|Rows present in both inputs|Same as UNION|`SELECT a FROM t1 INTERSECT SELECT a FROM t2`|
|**JOIN**|Combines columns based on a condition|2+ tables|Combined columns (cartesian product filtered by condition)|Join condition (e.g., `ON t1.id = t2.id`)|`SELECT * FROM t1 INNER JOIN t2 ON t1.id = t2.id`|
|**MERGE** (UPSERT)|Inserts, updates, or deletes rows based on a condition|Target + source table|Modified target table|Requires a key to match rows (DB-specific syntax)|`MERGE INTO target USING source ON (...) WHEN MATCHED THEN UPDATE...`|

The %sql and %python magic commands allow switching between languages

1. Vertical Scaling (Scale Up):
    - If your queries are running sequentially, meaning one query at a time, and you want to improve performance for a single query, you can scale up the cluster by increasing the size of the nodes.
    - For example, you can upgrade from a smaller instance type (e.g., 2x small) to a larger instance type (e.g., 4x large) to allocate more resources to the query. This can improve query execution time for individual queries.
2. Horizontal Scaling (Scale Out):
    - If your queries are running concurrently or there are multiple users running queries simultaneously, scaling out is a better approach. Scaling out involves adding more clusters to distribute the workload.
    - Instead of increasing the size of a single cluster, you can create multiple clusters and configure them to handle the workload in parallel.
    - Databricks provides features like Auto Scaling, which can automatically adjust the number of clusters based on workload demand. This helps ensure resources are efficiently allocated to handle the workload.
To summarize:
- Sequential queries: Scale up by increasing the size of the cluster to allocate more resources for better performance.
- Concurrent queries or multiple users: Scale out by adding more clusters to distribute the workload and enable parallel processing.

connection to SQLite using JDBC
```
CREATE TABLE users_jdbc
USING org.apache.spark.sql.jdbc
OPTIONS (
url = "jdbc:sqlite:/sqmple_db",
dbtable = "users"
)
```
