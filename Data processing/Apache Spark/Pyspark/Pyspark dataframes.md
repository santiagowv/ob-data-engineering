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
# Selecting columns
```python
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").show(2)
```
Output:
```
+-----------------+-------------------+ 
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME| 
+-----------------+-------------------+ 
| United States   | Romania           | 
| United States   | Croatia           | 
+-----------------+-------------------+
```
## Different ways of referring to columns
```python
from pyspark.sql.functions import expr, col, column

df.select(
	expr("DEST_COUNTRY_NAME"),
	col("DEST_COUNTRY_NAME"),
	column("DEST_COUNTRY_NAME")
).show(2)
```
Output:
```
+-----------------+-----------------+-----------------+
|DEST_COUNTRY_NAME|DEST_COUNTRY_NAME|DEST_COUNTRY_NAME|
+-----------------+-----------------+-----------------+
|    United States|    United States|    United States|
|    United States|    United States|    United States|
+-----------------+-----------------+-----------------+
```
## Referencing columns with expressions (expr)
It can refer to a <span style="color:rgb(216, 203, 251)">plain column or a string manipulation of a column</span>.
```python
df.select(expr("DEST_COUNTRY_NAME AS destination")).show(2)
```
### Shorthand selection
```python
df.selectExpr("DEST_COUNTRY_NAME AS newColumnName", "DEST_COUNTRY_NAME").show(2)
```
### Advanced expressions
```python
df.selectExpr(
	"*", # all original columns
	"(DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME) AS withinCountry"
).show(2)
```
Output:
```
+-----------------+-------------------+-----+-------------+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|withinCountry|
+-----------------+-------------------+-----+-------------+
|    United States|            Romania|   15|        false|
|    United States|            Croatia|    1|        false|
+-----------------+-------------------+-----+-------------+
```
### Genreating aggregations
```python
df.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```
Output:
```
+-----------+---------------------------------+
| avg(count)|count(DISTINCT DEST_COUNTRY_NAME)|
+-----------+---------------------------------+
|1770.765625|                              132|
+-----------+---------------------------------+
```
# Adding columns
## Add columns with select
```python
from pyspark.sql.functions import lit

df.select(expr("*"), lit(1).alias("One")).show(2)
```
Output:
```
+-----------------+-------------------+-----+---+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|One|
+-----------------+-------------------+-----+---+
|    United States|            Romania|   15|  1|
|    United States|            Croatia|    1|  1|
+-----------------+-------------------+-----+---+
```
## Add columns wiht withColumn
The `withColumn` function takes two arguments; the <span style="color:rgb(216, 203, 251)">column name and the expression</span> that will create the value.
```python
df.withColumn("numberOne", lit(1)).show(2)
```
Output:
```
+-----------------+-------------------+-----+---+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|One|
+-----------------+-------------------+-----+---+
|    United States|            Romania|   15|  1|
|    United States|            Croatia|    1|  1|
+-----------------+-------------------+-----+---+
```

```python
df.withColumn("withinCountry", expr("DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME")).show(2)
```
Output:
```
+-----------------+-------------------+-----+-------------+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|withinCountry|
+-----------------+-------------------+-----+-------------+
|    United States|            Romania|   15|        false|
|    United States|            Croatia|    1|        false|
+-----------------+-------------------+-----+-------------+
```
# Renaming columns
```PYTHON
df.withColumnRenamed("DEST_COUNTRY_NAME", "dest").columns
```
Output
```
['dest', 'ORIGIN_COUNTRY_NAME', 'count']
```
# Removing columns
```python
df.drop("DEST_COUNTRY_NAME").show(2)
```
Output:
```
+-------------------+-----+
|ORIGIN_COUNTRY_NAME|count|
+-------------------+-----+
|            Romania|   15|
|            Croatia|    1|
+-------------------+-----+
```
# Changing a column's type
```python
df.withColumn("count2", col("count").cast("long"))
```
# Filtering rows
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
## Advanced filter
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
## Filtering with or
```python
from pyspark.sql.functions import instr

priceFilter = col("UnitPrice") > 600
descripFilter = instr(df.Description, "POSTAGE") >= 1

df.where(df.StockCode.isin("DOT")).where(priceFilter | descripFilter).show()
```
## Filtering with a column definition
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
# Getting unique rows
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
# Sorting rows
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