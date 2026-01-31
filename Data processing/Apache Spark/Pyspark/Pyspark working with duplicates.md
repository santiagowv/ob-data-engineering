# Remove exact duplicate rows
## `dropDuplicates()`
```python
df_dedup = df.dropDuplicates()
```
## `distinct()`
```python
df_dedup = df.distinct()
```
# Remove duplicates based on specific columns
```python
df_dedup = df.dropDuplicates(["user_id", "event_date"])
```