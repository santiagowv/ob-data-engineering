# Write method
```python
df_with_metadata.write.format("delta").mode("overwrite").saveAsTable("gizmobox.bronze.customers")
```
## Format method
We can use the `noop` format to <span style="color:rgb(216, 203, 251)">prevent the write from executing when doing tests</span>.
```python
df_with_metadata.write.format("noop").mode("overwrite").save("../data/test.parquet")
```
# WriteTo method
```python
df_with_metadata.writeTo("gizmobox.bronze.customers").createOrReplace()
```
