# Create read stream
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.schema(customers_schema)\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```
## Cloud notification mode
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.option("cloudFiles.useNotifications", "true")\
					.schema(customers_schema)\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```
## Schema inference
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.option("cloudFiles.useNotifications", "true")\
					.option("cloudFiles.schemaLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_schema")\
					.option("cloudFiles.inferColumnTypes", "true")\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```
## Schema evolution
`cloudFiles.schemaEvolutionMode` options:
- `addNewColumns`: Stream fails. New <span style="color:rgb(216, 203, 251)">columns are added to the schema</span>.
- `rescue`: <span style="color:rgb(216, 203, 251)">Schema is never evolved</span> and stream does not fail due to schema changes. New columns are <span style="color:rgb(216, 203, 251)">recorded in the rescued data column</span>.
- `failOnNewColumns`: Stream fails. <span style="color:rgb(216, 203, 251)">Stream does not restart unless the provided schema is updated</span>, or the offending data file is removed.
- `none`: <span style="color:rgb(216, 203, 251)">Does not evolve the schema, new columns are ignored</span>. Stream does not fail due to schema changes.
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.option("cloudFiles.useNotifications", "true")\
					.option("cloudFiles.schemaLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_schema")\
					.option("cloudFiles.inferColumnTypes", "true")\
					.option("cloudFiles.schemaEvolutionMode", "addNewColumns")\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```
### Merged new schema
```python
streaming_query = customers_transformed_df.writeStream\
							.format("delta")\
							.option("checkpointLocation", "/Volumes/gizmobox/landing/operational_data/customers_autoloader/_checkpoint_stream")\
							.option("mergeSchema", "true")\
							.toTable("gizmobox.bronze.customers_autoloader")
```
## Schema hints
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.option("cloudFiles.useNotifications", "true")\
					.option("cloudFiles.schemaLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_schema")\
					.option("cloudFiles.inferColumnTypes", "true")\
					.option("cloudFiles.schemaEvolutionMode", "addNewColumns")\
					.option("cloudFiles.schemaHints", "date_of_birth DATE, member_since DATE, created_timestamp TIMESTAMP")\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```
# File options
## pathGlobFilter
<span style="color:rgb(216, 203, 251)">Filter files</span> by name patterns.
```python
customers_df = spark.readStream\
					.format("cloudFiles")\
					.option("cloudFiles.format", "json")\
					.option("cloudFiles.useNotifications", "true")\
					.option("cloudFiles.schemaLocation", "/Volumes/gizmobox/landing/operational_data/customers_stream/_schema")\
					.option("cloudFiles.inferColumnTypes", "true")\
					.option("cloudFiles.schemaEvolutionMode", "addNewColumns")\
					.option("cloudFiles.schemaHints", "date_of_birth DATE, member_since DATE, created_timestamp TIMESTAMP")\
					.option("pathGlobFilter", "customers_2024_*.json")\
					.load("/Volumes/gizmobox/landing/operational_data/customers_stream")
```