# Group by
## Sum
```python
df_pyspark.groupBy("Customer ID").sum().show()
```
## Count
```python
df_pyspark.groupBy("Customer ID").count().show()
```
# Agg
## Aggregate entire column
```python
df_pyspark.agg({"Price": "sum"}).show()
```
## Aggregate with Group By
```python
df_pyspark.groupBy("Customer ID").agg(max("date").alias("max_date"))
```