<span style="color:rgb(216, 203, 251)">Removes old, unsued files</span> from the Delta Lake to free up storage.
- Deletes files that are <span style="color:rgb(216, 203, 251)">no longer referenced in the transaction log</span>.
- Deletes files older than <span style="color:rgb(216, 203, 251)">retention threshold</span> (default 7 days).
- Reduces cloud <span style="color:rgb(216, 203, 251)">storages costs</span>.
- Improves <span style="color:rgb(216, 203, 251)">query performance</span>.
- Helps <span style="color:rgb(216, 203, 251)">comply with privacy regulations</span>.

```SQL
SET spark.databricks.delta.retentionDurationCheck.enabled = false;
VACUUM demo.delta_lake.optimize_stock_prices RETAIN 0 HOURS;
```