```
spark.readStream \
.table("transactions") \
.withColumn("transaction_value", col("amount") / col("quantity")) \
.writeStream \
.option("checkpointLocation", "checkpointPath") \
.outputMode("complete") \
.table("processed_transactions")
```

```
spark\
.readStream\
.format("cloudFiles")\ #using auto loader, only if format is cloudfiles
.option("cloudFiles.format","csv")\
.option("cloudFiles.schemaLocation", checkpoint_directory)\
.load("landing")\
.writeStream.option("checkpointLocation", checkpoint_directory)\
.table(raw)
```

```
CREATE STREAMING LIVE TABLE loyal_customers
AS SELECT customer_id
FROM STREAM(LIVE.customers)
WHERE loyalty_level = 'high';
```

| Trigger                             | Frequency                  | Case                              | Type                       |
| ----------------------------------- | -------------------------- | --------------------------------- | -------------------------- |
| trigger(once=True)                  | Once                       | Deterministic one-time processing | batch-like, auto-stop      |
| trigger(processingTime='5 seconds') | Fixed interval             | Regular interval processing       | micro-batch, not auto-stop |
| trigger(availableNow=true)          | Continuous until caught up | Catch-up processing               | micro-batch, auto-stop     |
| trigger()                           | As soon as data arrives    | Real-time processing              | micro-batch, not auto-stop |

Auto Loader
 Auto Loader is designed to work with Structured Streaming for efficient data ingestion

| Feature             | Purpose                               | Focus               | Use case                                   | Advantages                                  | Disadvantages                        |
| ------------------- | ------------------------------------- | ------------------- | ------------------------------------------ | ------------------------------------------- | ------------------------------------ |
| Checkpointing       | Fault Tolerance via state persistence | Metada/state        | Stateful operations, failure recovery      | Ensures fault tolerance, no data loss       | Adds I/O overhead                    |
| Watermarking        | Manage late-arriving data             | Windowed operations | Time-based aggreagtions with delays        | Prevents unbounded state growth             | Can discard valid late data          |
| Write-Ahead Logging | Fault tolerance for input data        | Input data          | Durable processing with unreliable sources | Guarantees data durability                  | Adds write latency, storage overhead |
| Idempotent Sinks    | Prevent duplicate writes              | Output consistency  | Exactly-once deilvery to sinks             | Prevents duplicate writes, ensures accuracy | Requires sink compatibility          |
