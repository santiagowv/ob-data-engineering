# Format columns with date and timestamp
```python
from pyspark.sql.functions import current_date, current_timestamp

date_df = spark.range(10)\
	.withColumn("today", current_date())\
	.withColumn("now", current_timestamp())

date_df = createOrReplaceTempView("dataTable")

date_df.printSchema()
```
Output:
```
|-- id: long (nullable = false)
|-- today: date (nullable = false)
|-- now: timestamp (nullable = false)
```
# Add and substract days
```python
from pyspark.sql.functions import date_add, date_sub

date_df.select(date_sub(col("today"), 5), date_add(col("today"), 5)).show(1)
```
Output:
```
+------------------+------------------+
|date_sub(today, 5)|date_add(today, 5)|
+------------------+------------------+
|        2022-11-14|        2022-11-24|
+------------------+------------------+
```
# Difference between dates
```python
from pyspark.sql.functions import datediff, col, months_between, to_date

date_df.withColumn("week_ago", date_sub(col("today"), 7))\
	.select(datediff(col("week_ago"), col("today"))).show(1)
	
date_df.select(
	to_date(lit("2016-01-01")).alias("start"),
	to_date(lit("2017-05-22")).alias("end"))\
	.select(months_between(col("start"), col("end"))).show(1)
```
Output:
```
+-------------------------+
|datediff(week_ago, today)|
+-------------------------+
|                       -7|
+-------------------------+

+--------------------------------+
|months_between(start, end, true)|
+--------------------------------+
|                    -16.67741935|
+--------------------------------+
```
# Format date columns
```python
from pyspark.sql import functions as F

df.select(
		'payment_id',
		F.date_format('payment_timestamp', 'yyyy-MM-dd').cast('date').alias('payment_date'),
		F.date_format('payment_timestamp', 'HH-mm-ss').alias('payment_time')	
)
```