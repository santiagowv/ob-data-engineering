We can <span style="color:rgb(216, 203, 251)">register any DataFrame as a table or view</span> (temporary table) and query it using SQL.
# Create a table or view
```python
flight_data.createOrReplaceTempView("flight_data")
```
# Querying data
Querying data with SQL <span style="color:rgb(216, 203, 251)">returns a DataFrame</span>.
```python
sqlway = spark.sql("""
	SELECT DEST_COUNTRY_NAME, count(1)
	FROM flight_data
	GROUP BY DEST_COUNTRY_NAME
""")

dataframeway = flight_data\
	.groupBy("DEST_COUNTRY_NAME")\
	.count()
```