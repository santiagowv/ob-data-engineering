A Delta table is stored as a <span style="color:rgb(216, 203, 251)">collection of Parquet data files plus a Delta transaction log</span>. When a table contains too many small files, query performance can degrade because it must:
- List and plan <span style="color:rgb(216, 203, 251)">work for many files</span>.
- <span style="color:rgb(216, 203, 251)">Read file-level metadata</span> and parquet footers for many files.
- Schedule <span style="color:rgb(216, 203, 251)">many small tasks across executors</span>.
- Many small storage I/O requests rather than fewer, <span style="color:rgb(216, 203, 251)">larger sequential reads</span>.
# Common root causes of small files
## Excessive output partitions
10 GB of data -> 10.000 partitions.
- This often happens after an <span style="color:rgb(216, 203, 251)">unnecessary</span> `repartition(10000)`.
- Or when an upstream operation creates <span style="color:rgb(216, 203, 251)">far more shuffle partitions than the data volume justifies</span>.
## Partitioning by a high-cardinality column
High-cardinality columns spread data across too many partitions, which <span style="color:rgb(216, 203, 251)">leads to small-file proliferation</span>.
## Frecuent small appends or streaming micro-batches
Every 30 seconds -> 5-20 MB Written.
- This is common with Auto Loader, Structured streaming, API ingestion, and CDC pipelines.
## Frequent row-level updates and merges
`MERGE`, `UPDATE`, and `DELETE` can <span style="color:rgb(216, 203, 251)">rewrite subsets of exiting files</span>. Repeated small changes over time can <span style="color:rgb(216, 203, 251)">fragment the table into many files</span>.
# Optimize
Maintenance command that rewrites many small files into <span style="color:rgb(216, 203, 251)">fewer, larger files using a bin-packing algorithm</span>.
- Groups smaller files into bins and <span style="color:rgb(216, 203, 251)">rewrites each bin as a larger Parquet file</span>.
```sql
OPTIMIZE main.silver.claims;
```
- For a partitionied table, we can optimize only a targeted subset.
```sql
OPTIMIZE main.silver.claims
WHERE load_date >= '2026-06-01';
```
## Optimize execution process
The default target file size for `OPTIMIZE` is 1 GB.
- <span style="color:rgb(216, 203, 251)">Reads existing active</span> Delta files.
- <span style="color:rgb(216, 203, 251)">Groups small files</span> into larger target-size bins.
- Writes <span style="color:rgb(216, 203, 251)">replacement Parquet files</span>.
- <span style="color:rgb(216, 203, 251)">Commits a new Delta transaction</span> that marks the old files as removed.
- <span style="color:rgb(216, 203, 251)">Leaves the old physical files in cloud storage</span> until they are eligible for deletion by `VACUUM`.
## When to use optimize
- The table already has a <span style="color:rgb(216, 203, 251)">significant accumulation of small files</span>.
- Table receives many `MERGE`, `UPDATE`, or `DELETE` operations.
- A streaming or micro-batch <span style="color:rgb(216, 203, 251)">pipeline has produced fragmented files over time</span>.
- We want to apply `ZORDER` or liquid clustering.
- We maintain a large, frequently quiered silver or gold table.
# Optimize write
Write-time optimization that attempts to <span style="color:rgb(216, 203, 251)">prevent small files from being created in the first place</span>.
- Databricks may <span style="color:rgb(216, 203, 251)">shuffle and rebalance the data before </span>writing so that the resulting files are closer to an efficient target size.
- Happens <span style="color:rgb(216, 203, 251)">during the write operation</span>.
- Most beneficial for partitioned tables because it <span style="color:rgb(216, 203, 251)">reduces the number of small files written to each partition</span>.
- May <span style="color:rgb(216, 203, 251)">introduce a shuffle</span>.
- Can increase write <span style="color:rgb(216, 203, 251)">latency and compute cost</span>.
- Often removes the need for manual `coalesce(n)` or `repartition(n)` immediately before a write.
![[Pasted image 20260620193326.png]]
```sql
SET TBLPROPERTIES ( 'delta.autoOptimize.optimizeWrite' = 'true' );
```
# Auto compaction
It's a post-write optimization that <span style="color:rgb(216, 203, 251)">fixes small files created by a recent write</span>.
- If enough files exist, <span style="color:rgb(216, 203, 251)">it rewrites them into fewer, larger parquet files bin packing</span>.
- Happens after a successful write.
- Runs <span style="color:rgb(216, 203, 251)">synchronously using the compute that performed the write</span>.
- Can <span style="color:rgb(216, 203, 251)">add latency to the write job</span> because compaction happens before the job fully completes.
- Compacts only <span style="color:rgb(216, 203, 251)">recent or affected files</span> rather than scanning the entire table.
- Is useful for <span style="color:rgb(216, 203, 251)">continuous small updates</span>, micro-batch ingestion, and streaming workloads.
Useful to fix the small file problem when a table is receiving small updates.
- We can specify the number of files that need to be present to trigger auto-compaction.
- Databricks checks the files created or affected in the relevant table partition(s). When it detects enough small files, it **bin-packs them into fewer, larger Parquet files**.
```python
spark.conf.set( "spark.databricks.delta.autoCompact.minNumFiles", "50" )
```