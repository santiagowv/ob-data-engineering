# Column operations
```python
from pyspark.sql.functions import expr, pow

fabricatedQuantity = pow(col("Quantity") * col("UnitPrice"), 2) + 5

df.select(expr("CustomerId"), fabricatedQuantity.alias("realQuantity")).show(2)
```
Or with SQL expressions:
```python
df.selectExpr("CustomerId", "(POWER((Quantity * UnitPrice), 2.0) + 5) as realQuantity").show(2)
```
Output:
```
+----------+------------------+
|CustomerId|      realQuantity|
+----------+------------------+
|   17850.0|239.08999999999997|
|   17850.0|          418.7156|
+----------+------------------+
```
# Rounding
The `round` function rounds up by default.
## Round up
```python
from pyspark.sql.functions import round

df.select(round(col("UnitPrice"), 1).alias("rounded"), col("UnitPrice")).show(5)
```
Output:
```
Output:
+-------+---------+
|rounded|UnitPrice|
+-------+---------+
|    2.6|     2.55|
|    3.4|     3.39|
|    2.8|     2.75|
|    3.4|     3.39|
|    3.4|     3.39|
+-------+---------+
```
## Round down
```python
from pyspark.sql.functions import round, bround

df.select(round(lit("2.5")), bround(lit("2.5"))).show(2)
```
Output:
```
+-------------+--------------+
|round(2.5, 0)|bround(2.5, 0)|
+-------------+--------------+
|          3.0|           2.0|
|          3.0|           2.0|
+-------------+--------------+
```
# Pearse correlation
```python
from pyspark.sql.functions import corr

df.select(corr("Quantity", "UnitPrice")).show()
```
Output:
```
+-------------------------+
|corr(Quantity, UnitPrice)|
+-------------------------+
|     -0.04112314436835551|
+-------------------------+
```
# Describe
```python
df.describe()
```
Output:
```
DataFrame[summary: string, InvoiceNo: string, StockCode: string, Description: string, Quantity: string, InvoiceDate: string, UnitPrice: string, CustomerID: string, Country: string]
```
# Manual aggregation functions
```python
from pyspark.sql.functions import count, mean, stddev_pop, min, max
```
# Dataframe stat method
There are a number of stats functions using the dataframe `stat` method.
## Crosstab
```python
df.stat.crosstab("StockCode", "Quantity")
```