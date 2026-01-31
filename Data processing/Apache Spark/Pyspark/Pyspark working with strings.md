# Lower and uppercase
```python
from pyspark.sql.functions import initcap, lower, upper, col

df.select(initcap(col("Description")),
		  lower(col("Description")),
		  upper(col("Description"))).show()
```
# Trimming
We can do this by using:
- `lpad`
- `rpad`
- `ltrim`
- `rtrim`
```python
from pyspark.sql.functions import lit, ltrim, rtrim, rpad, lpad, trim

df.select(
	ltrim(lit(" HELLO ")).alias("ltrim"), 
	rtrim(lit(" HELLO ")).alias("rtrim"), 
	trim(lit(" HELLO ")).alias("trim"), 
	lpad(lit("HELLO"), 3, " ").alias("lp"), 
	rpad(lit("HELLO"), 10, " ").alias("rp") 
).show(2)
```
# Regular expressions
We have two functions to perform regular expression tasks:
- `regexp_extract`: <span style="color:rgb(216, 203, 251)">extract</span> values.
- `regexp_replace`<span style="color:rgb(216, 203, 251)">replace</span> values.
## Substitute based on character criteria
```python
from pyspark.sql.functions import regexp_replace, col

regex_string = "BLACK|WHITE|RED|GREEN|BLUE"
df.select(
	regexp_replace(col("Description"), regex_string, "COLOR").alias("color_clean"),
	col("Description")	
).show(2)
```
Output:
```
+--------------------+--------------------+
|         color_clean|         Description|
+--------------------+--------------------+
|COLOR HANGING HEA...|WHITE HANGING HEA...|
| COLOR METAL LANTERN| WHITE METAL LANTERN|
+--------------------+--------------------+
```
## Replace given characters with other characters
```python
from pyspark.sql.functions import translate, col

df.select(translate(col("Description"), "LEFT", "1337"), col("Description")).show(2)
```
## Check existence of a value
```python
from pyspark.sql.functions import instr, col

contains_black = instr(col("Description"), "BLACK") >= 1
contains_white = instr(col("Description"), "WHITE") >= 1
df.withColumn("hasSimpleColor", contains_black, contains_white)\
	.where("hasSimpleColor")\
	.select("Description").show(3)
```