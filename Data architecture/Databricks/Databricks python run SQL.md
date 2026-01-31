# Execute SQL commands with Python
```python
df = spark.sql('SELECT * FROM json.`/Volumes/gizmobox/landing/operational_data/customers`')
display(df)
```