# Write method
```python
df_with_metadata.write.format("delta").mode("overwrite").saveAsTable("gizmobox.bronze.customers")
```
# WriteTo method
```python
df_with_metadata.writeTo("gizmobox.bronze.customers").createOrReplace()
```