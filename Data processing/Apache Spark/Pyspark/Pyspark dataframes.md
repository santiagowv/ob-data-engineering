Represents a <span style="color:rgb(216, 203, 251)">table of data with rows and columns</span>. The list that defines the <span style="color:rgb(216, 203, 251)">columns and the types within those columns is called the schema</span>.
# Components
## Schemas
- Define the <span style="color:rgb(216, 203, 251)">name</span> as well as the <span style="color:rgb(216, 203, 251)">type of data</span> in each column.
## Partitioning
- Layout of the <span style="color:rgb(216, 203, 251)">DataFrame or Dataset's physical distribution across the cluster</span>.
- The partitioning scheme defines how that is allocated. 
	- We can set this to be based on values in a certain column or nondeterministically.
# Manually create a DataFrame
```python
from pyspark.sql import Row
from pyspark.sql.types import StructField, StringType, LongType

manual_schema = StructType([
	StructField("some", StringType(), True),
	StructField("col", StringType(), True),
	StructField("names", LongType(), False)
])

my_row = Row("Hello", None, 1)
my_df = spark.createDataFrame([my_row], manual_schema)
my_df.show()
```
Output:
```
+-----+----+-----+ 
| some| col|names| 
+-----+----+-----+ 
|Hello|null| 1   | 
+-----+----+-----+
```
# Filter rows
## Simple filter
```python
df.filter(col("count") < 2).show(2)
# or
df.where("count < 2")
```
Output:
```
+-----------------+-------------------+-----+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|
+-----------------+-------------------+-----+
|    United States|            Croatia|    1|
|    United States|          Singapore|    1|
+-----------------+-------------------+-----+
```
## Where filter
<span style="color:rgb(216, 203, 251)">Chaining multiple filters</span> sequentially.
```python
df.where(col("count") < 2).where(col("ORIGIN_COUNTRY_NAME") != "Croatia").show(2)
```
Output:
```
+-----------------+-------------------+-----+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|
+-----------------+-------------------+-----+
|    United States|          Singapore|    1|
|          Moldova|      United States|    1|
+-----------------+-------------------+-----+
```
## SQL filter
```python
df.filter('customer_id is not null')
```
## Filter with operators
### Or operator
```python
from pyspark.sql.functions import instr

priceFilter = col("UnitPrice") > 600
descripFilter = instr(df.Description, "POSTAGE") >= 1

df.where(df.StockCode.isin("DOT")).where(priceFilter | descripFilter).show()
```
### And operator
```python
from pyspark.sql.functions import instr

priceFilter = col("UnitPrice") > 600
descripFilter = instr(df.Description, "POSTAGE") >= 1

df.where(df.StockCode.isin("DOT")).where(priceFilter & descripFilter).show()
```
### Not operator
```python
df_pyspark.filter(~((df_pyspark["Sales"] > 30) & (df_pyspark["Frequency"] < 5))).show()
```
## Filter with a column definition
```python
from pyspark.sql.functions import instr

DOTCodeFilter = col("StockCode") == "DOT"
priceFilter = col("UnitPrice") > 600
descripFilter = instr(col("Description"), "POSTAGE") >= 1

df.withColumn("isExpensive", DOTCodeFilter & (priceFilter | descripFilter))\
  .where("isExpensive")\
  .select("unitPrice", "isExpensive").show(5)
```
Outut:
```
+---------+-----------+
|unitPrice|isExpensive|
+---------+-----------+
|   569.77|       true|
|   607.49|       true|
+---------+-----------+
```
## Filter based on date
```python
date_df.filter(col("today") > "2022-12-12").show()
```
## Filter with null values
We use object notation.
```python
df_filtered = df.filter(df.customer_id.isNotNull())
```
# Get unique rows
```python
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").distinct().count()
```
# Sampling
```python
seed = 5
withReplacement = False
fraction = 0.5

df.sample(withReplacement, fraction, seed).count()
```
## Random splits
```python
dataframes = df.randomSplit([0.25, 0.75], seed)
print(dataframes[0].count())
print(dataframes[1].count())
```
# Append a new dataframe
<span style="color:rgb(216, 203, 251)">Both dataframes must have the same schema</span> and number of columns; otherwise, the union will fail.
```python
from pyspark.sql import Row

schema = df.schema
new_rows = [
	Row("New Country", "Other Country", 5),
	Row("New Country 2", "Other Country 2", 1)
]

parallelized_rows = spark.sparkContext.parallelize(new_rows)
new_df = spark.createDataFrame(parallelized_rows, schema)

df.union(new_df).show(2)
```
Output:
```
+-----------------+-------------------+-----+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|
+-----------------+-------------------+-----+
|    United States|            Romania|   15|
|    United States|            Croatia|    1|
+-----------------+-------------------+-----+
```
# Sort rows
There are two equivalent functions to sort:
- `sort`
- `orderBy`
```python
from pyspark.sql.functions import col

df.sort("count").show(5)
# or
df.orderBy("count", "DEST_COUNTRY_NAME").show(5)
# or
df.orderBy(col("count"), col("DEST_COUNTRY_NAME")).show(5)
```
## Order of sorting
```python
from pyspark.sql.functions import desc, asc, expr
df.orderBy(expr("count desc")).show(2)
df.orderBy(col("count").desc(), col("DEST_COUNTRY_NAME").asc()).show(2)
```
# Limit
Starts from <span style="color:rgb(216, 203, 251)">top to bottom</span>.
```python
df.limit(5).show()
```