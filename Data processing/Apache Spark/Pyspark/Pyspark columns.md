We <span style="color:rgb(216, 203, 251)">can't manipulate an individual column</span> outside the context of a DataFrame; we must use Spark transformations within a DataFrame to modify the contents of a column.
# Construct a column
```python
from pyspark.sql.functions import col, column

col("someColumnName")
column("someColumnName")
```
# Select columns
```python
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").show(2)
```
## Different ways of referring to columns
```python
from pyspark.sql.functions import expr, col, column

df.select(
	"DEST_COUNTRY_NAME",
	expr("DEST_COUNTRY_NAME"),
	col("DEST_COUNTRY_NAME"),
	column("DEST_COUNTRY_NAME")
)
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
### Genreating aggregations
```python
df.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```
# Add columns
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
## Add metadata columns
```python
df_with_metadata = df.select("_metadata.file_path", "*")
```
## Add conditional columns
```python
df_extracted_payments.select(
						'payment_id',
						'order_id',
						'payment_date',
                        'payment_time',
                        F.when(df_extracted_payments.payment_status == 1, 'Success')
						 .when(df_extracted_payments.payment_status == 2, 'Pending')
                         .when(df_extracted_payments.payment_status == 3, 'Cancelled')
                         .when(df_extracted_payments.payment_status == 4, 'Failed')
                         .alias('payment_status'),
                         'payment_method'
)
```
# Rename columns
```PYTHON
df.withColumnRenamed("DEST_COUNTRY_NAME", "dest").columns
```
Output
```
['dest', 'ORIGIN_COUNTRY_NAME', 'count']
```
# Remove columns
```python
df.drop("DEST_COUNTRY_NAME")
```
# Split columns by separator
```python
df.select(
	"refund_id",
	"payment_id",
	"refund_timestamp",
	"refund_amount",
    F.split("refund_reason", ":")[0].alias("refund_reason"),
    F.split("refund_reason", ":")[1].alias("refund_source")
)
```
# Regular expressions
## Extract data
```python
df.select(
	F.regexp_extract("refund_reason", "^([^:]+):", 1).alias("refund_reason"),
    F.regexp_extract("refund_reason", "^[^:]+:(.*)$", 1).alias("refund_source")
)
```

```python
df_memberships.select(
				F.regexp_extract("path", r".*/([0-9]+)\.png", 1).alias("customer_id"),
				F.col("content").alias("membership_card")
)
```