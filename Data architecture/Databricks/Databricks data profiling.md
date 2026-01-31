We use the dbutils object in a Dataframe.
```python
df = spark.table("gizmobox.bronze.v_customers")
dbutils.data.summarize(df)
```