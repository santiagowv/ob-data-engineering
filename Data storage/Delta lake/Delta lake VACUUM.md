<span style="color:rgb(216, 203, 251)">Removes old, unsued files</span> from the Delta Lake to free up storage.
- Deletes files that are <span style="color:rgb(216, 203, 251)">no longer referenced in the transaction log</span>.
- Deletes files older than <span style="color:rgb(216, 203, 251)">retention threshold</span> (default 7 days).
- Reduces cloud <span style="color:rgb(216, 203, 251)">storages costs</span>.
- Improves <span style="color:rgb(216, 203, 251)">query performance</span>.
- Helps <span style="color:rgb(216, 203, 251)">comply with privacy regulations</span>.
![[Drawing 2026-07-05 18.43.39.excalidraw]]
# Use VACUUM
- Disable the safety check
```sql
SET spark.databricks.delta.retentionDurationCheck.enabled = false;
```
- Delete files older than 1 hour.
```sql
VACUUM main.sales.orders RETAIN 1 HOURS;
```