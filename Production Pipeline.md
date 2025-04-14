https://www.databricks.com/glossary/medallion-architecture

https://docs.databricks.com/aws/en/jobs/monitor

https://docs.databricks.com/en/sql/user/alerts/index.html

https://docs.databricks.com/en/compute/pool-index.html

query editor > schedule

Cluster pools are a way to pre-provision clusters that are ready to use. This can reduce the start up time for clusters, as they do not have to be created from scratch.
Job clusters are ephemeral and designed for running production jobs efficiently.
All-purpose clusters are designed for interactive and exploratory workloads, not optimized for production jobs.
Single-node clusters are the smallest type of cluster, and they will start up the fastest. However, they may not be powerful enough to run the Job's tasks.
Autoscaling clusters can scale up or down based on demand. This can help to improve the start up time for clusters, as they will only be created when they are needed. However, autoscaling clusters can also be more expensive than other types of cluster pool

Summary of common issues :
- Cold start => Serverless
- Reduce cost => Auto stop
- Throughput / Sequential => Scale Up / Cluster Size
- Performance / Concurrent => Scale Out / Scaling
PS : **SQL endpoints** has been renamed to **SQL warehouses**

CONSTRAINT *x* EXPECT (*condition*)
- ON VIOLATION FAIL : fail update and drop all rows
- ON VIOLATION DROP ROW : add other rows and flag bad rows
- omit : add all rows but flag bad rows

 OPTIMIZE consolidates small files into larger files for better performance.
 VACUUM removes old data files but does not optimize small files.