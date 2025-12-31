We <span style="color:rgb(216, 203, 251)">can't manipulate an individual column</span> outside the context of a DataFrame; we must use Spark transformations within a DataFrame to modify the contents of a column.
# Construct a column
```python
from pyspark.sql.functions import col, column

col("someColumnName")
column("someColumnName")
```
# Accessing a DataFrame's column
```python
df = spark.read.format("json")\
		  .load("abfss://spark-training@stsparkproject.dfs.core.windows.net/flight-data/2015-summary.json")
		  
df.columns
```
Output:
```
['DEST_COUNTRY_NAME', 'ORIGIN_COUNTRY_NAME', 'count']
```
# Selecting columns
## Option 1
```python
df.select("count")
```
Output:
```
DataFrame[count: bigint]
```
## Option 2
```python
from pyspark.sql.functions import col

df.select(col("count"))
```
Output:
```
DataFrame[count: bigint]
```