<span style="color:rgb(216, 203, 251)">The data object is only mentioned in the query</span>, not as an argument. There isn't a local object in the environment that holds the data.
```python
query = "FROM flights SELECT * LIMIT 10"

flights10 = spark.sql(query)

flights10.show()
```