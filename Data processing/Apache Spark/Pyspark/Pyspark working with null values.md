# Remove null values
```python
df_pyspark = df_pyspark.na.drop()
```
## Remove strategy
If there's <span style="color:rgb(216, 203, 251)">at least one column with null</span> deletes the entire row:
```python
# if there's at least one column with null
df_pyspark = df_pyspark.na.drop(how = "Any")
```
If <span style="color:rgb(216, 203, 251)">every column has null values</span> deletes that row:
```python
# if every column has null values
df_pyspark = df_pyspark.na.drop(how = "All")
```
## Remove threshold
At least <span style="color:rgb(216, 203, 251)">two non null values</span> are accepted and the row won't be removed:
```python
df_pyspark = df_pyspark.na.drop(how = "Any", thresh = 2)
```
## Subset
Remove <span style="color:rgb(216, 203, 251)">null values from columns passed in subset parameter</span>.
```python
df_pyspark = df_pyspark.na.drop(how = "Any", subset = ["Sales"])
```
# Fill missing values
```python
df_pyspark.na.fill(200, subset = ["Frequency"]).show()
```