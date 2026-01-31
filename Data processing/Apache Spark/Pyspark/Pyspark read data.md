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
## Read data with format method
```python
df = spark.read.format("json").load("path")
```
## Read data with file type method
```python
df = spark.read.json("path")
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
### DDL
```python
payment_schema = 'payment_id INTEGER, order_id INTEGER, payment_timestamp TIMESTAMP, payment_status INTEGER, payment_method STRING'

df = (
	spark.read.format("csv")
		 .option("delimiter", ",")
		 .schema(payment_schema)
		 .load("path")
)
```
### Schema methods
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
## Get schema
```python
df_pyspark.printSchema()
```
# Get data types
```python
df_pyspark.dtypes
```