# Read CSV
```python
df = spark.read.json('/Volumes/gizmobox/landing/operational_data/customers')
```
Or
```python
df = spark.read.format('json').load('/Volumes/gizmobox/landing/operational_data/customers')
```
# Read data from catalog
```python
df = spark.table('gizmobox.bronze.v_addresses')
```