Process of <span style="color:rgb(216, 203, 251)">combining many smaller files into few larger files</span> to improve performance and optimize storage.
- Small file problem.
- Faster reads.
- Efficient storage.
# Types of compaction
## File compaction
<span style="color:rgb(216, 203, 251)">Rewrites many small parquet files</span> into fewer, larger files.
```sql
OPTIMIZE demo.delta_lake.optimize_stock_prices;
```
## Z-ORDER compaction
Rewrites files and <span style="color:rgb(216, 203, 251)">co-locates similar values together</span> across one or more columns using Z-ordering, which can <span style="color:rgb(216, 203, 251)">improve data skipping for filers on those columns</span>.
```sql
OPTIMIZE demo.delta_lake.optimize_stock_prices
ZORDER BY stock_id;
```
## Liquid clustering
<span style="color:rgb(216, 203, 251)">Defines clustering keys for a table</span>, and <span style="color:rgb(216, 203, 251)">OPTIMIZE</span> incrementally rewrites files to group data by those keys.
- <span style="color:rgb(216, 203, 251)">Data is divided into clusters</span> based on the clustering keys.
- As new data arrives, databricks monitors and <span style="color:rgb(216, 203, 251)">rebalances these clusters automatically</span>.
- Changing the clustering columns <span style="color:rgb(216, 203, 251)">doesn't require re-writing entire data</span>.
- <span style="color:rgb(216, 203, 251)">No manual</span> re-organization or z-ordering required.
```sql
CREATE TABLE table1
( col1 INT, col2 STRING)
CLUSTER BY (col1);
```

```sql
ALTER TABLE table1
CLUSTER BY (col1);
```
# Choose compaction strategy
- **File compaction:** if we're dealing with <span style="color:rgb(216, 203, 251)">too many small files</span>.
- **z-order:** if queries always <span style="color:rgb(216, 203, 251)">filter by the same columns and we want strong skipping</span>.
- **Liquid clustering:** If <span style="color:rgb(216, 203, 251)">filter/queries evolve a lot</span> or partitioning choices keep changing.
