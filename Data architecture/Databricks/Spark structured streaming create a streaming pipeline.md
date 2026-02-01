![[Pasted image 20260201175122.png]]
# Read files using DataStreamReader API
<span style="color:rgb(216, 203, 251)">Schema inference is disabled by default</span> to avoid inconsistencies.
## Create the read schema
```python
from pyspark.sql.types import StructType, StructField, IntegerType, StringType, DateType, TimestampType

customers_schema = StructType(fields = [StructField("customer_id", IntegerType()),
                                        StructField("customer_name", StringType()),
                                        StructField("date_of_birth", DateType()),
                                        StructField("telephone", StringType()),
                                        StructField("email", StringType()),
                                        StructField("member_since", DateType()),
                                        StructField("created_timestamp", TimestampType())])
```
## Create read stream
```python
spark.readStream
	.format("json")
	.schema(customers_schema)
	.load("/Volumes/gizmobox/landing/operational_data/customers_stream/")
```
# Transform stream
It <span style="color:rgb(216, 203, 251)">runs for all new data</span>. 
```python
from pyspark.sql import functions as F

customers_transformed_df = customers_df.withColumn("file_path", F.col("_metadata.file_path"))\
                                       .withColumn("ingestion_date", F.current_timestamp())
```
# Write stream
<span style="color:rgb(216, 203, 251)">Starts the streaming pipeline</span> and provides metrics..
```python
streaming_query = customers_transformed_df.writeStream\
                                            .format("delta")\
                                            .option("checkpointLocation" , "/Volumes/gizmobox/landing/operational_data/customers_stream/_checkpoint_stream")\                                        .toTable("gizmobox.bronze.customers_stream")
```
# Trigger
Method to <span style="color:rgb(216, 203, 251)">invoke the data micro-batches</span> at a specific interval.

| Trigger type         | Trigger syntax                           | Description                                                                       |
| -------------------- | ---------------------------------------- | --------------------------------------------------------------------------------- |
| Default (No trigger) | No trigger specified                     | 500 microseconds interval                                                         |
| Fixed interval       | `.trigger(processingTime = "2 minutes")` | Micro batch starts at the user specified interval                                 |
| Triggered once       | `.trigger(once = True`)                  | Processes all data available as one micro-batch and stops [Deprecated]            |
| Available now        | `.trigger(availableNow = True)`          | Processes all data available as multiple micro-batch and stops                    |
| Continuous           | `.trigger(continuous = "2 seconds")`     | Porcesses data continuously, but checkpoints at interval specified [Experimental] |
```python
streaming_query = customers_transformed_df.writeStream\
                                            .format("delta")\
                                            .trigger(processingTime = "2 minutes")\
                                            .option("checkpointLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_checkpoint_stream")\                                        .toTable("gizmobox.bronze.customers_stream")
```
# outputMode
Controls how the <span style="color:rgb(216, 203, 251)">processed data is written to the sink</span>.

| Output mode      | Description                                                       |
| ---------------- | ----------------------------------------------------------------- |
| Append (Default) | Writes the new rows arrived since the last micro-batch            |
| Complete         | Writes the entire result to the sink every time                   |
| Update           | Wirtes only the rows that have changed since the last micro-batch |
```python
```python
streaming_query = customers_transformed_df.writeStream\
                                            .format("delta")\
                                            .outputMode("append")\
                                            .trigger(processingTime = "2 minutes")\
                                            .option("checkpointLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_checkpoint_stream")\                                        .toTable("gizmobox.bronze.customers_stream")
```
```