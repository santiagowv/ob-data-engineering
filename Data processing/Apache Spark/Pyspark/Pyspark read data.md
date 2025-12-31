# Read data
The number of rows in a Dataframe is unspecified because <span style="color:rgb(216, 203, 251)">reading data is a transformation</span>, and is therefore a lazy operation.
![[Pasted image 20251225164413.png|600]]
```python
flight_data = spark\
	.read\
	.option("inferSchema", "true")\
	.option("header", "true")\
	.csv("abfss://spark-training@stsparkproject.dfs.core.windows.net/flight-data/2015-summary.csv")
```
# Schema configuration
```python
df = spark.read.format("json")\
		  .load("abfss://spark-training@stsparkproject.dfs.core.windows.net/flight-data/2015-summary.json")
		  
df.schema
```
Output:
```
Output: StructType(List(StructField(DEST_COUNTRY_NAME,StringType,true),StructField(ORIGIN_COUNTRY_NAME,StringType,true),StructField(count,LongType,true)))
```
## Schema explicit definition
```python
from pyspark.sql.types import StructField, StructType, StringType, LongType

manual_schema = StructType([
	StructField("DEST_COUNTRY_NAME", StringType(), True),
	StructField("ORIGIN_COUNTRY_NAME", StringType(), True),
	StructField("count", LongType(), False, metadata = {"hello":"world"})
])

df = spark.read.format("json")\
		  .schema(manual_schema)\
		  .load("abfss://spark-training@stsparkproject.dfs.core.windows.net/flight-data/2015-summary.json")
		  
df.schema
```
Output:
```
Output: StructType(List(StructField(DEST_COUNTRY_NAME,StringType,true),StructField(ORIGIN_COUNTRY_NAME,StringType,true),StructField(count,LongType,true)))
```