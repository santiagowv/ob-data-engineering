Each row in a DataFrame is a single record. A record is an <span style="color:rgb(216, 203, 251)">object of type Row</span>.
- Spark <span style="color:rgb(216, 203, 251)">manipulates Row objects using column expressions</span> in order to produce usable values.
# Accessing rows
```python
df.first()
```
Output:
```
Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Romania', count=15)
```
# Creating rows
```python
from pyspark.sql import Row

myrow = Row("Hello", None, 1, False)
myrow
```
Output:
```
<Row('Hello', None, 1, False)>
```
# Accessing vlaues
```python
myrow[0]
```
Output:
```
'Hello'
```