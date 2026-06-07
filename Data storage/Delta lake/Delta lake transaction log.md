The `_delta_log` is the heart of ACID guarantees.
- Each file is a commit. A list of actions that describe what changed.
- Delta log contains metadata data like `min` and `max` values that can be used for <span style="color:rgb(216, 203, 251)">partition pruning</span>. 
- <span style="color:rgb(216, 203, 251)">Readers will always read the transaction logs first</span> to identify the list of data files to read.
# Insert
New files only, no mutations.
1. Writes one or more new parquet files.
2. Appends a commit JSON with `add` actions pointing to those files.
![[Drawing 2026-06-06 18.14.30.excalidraw]]
# Delete
Rewrite affected files, mark old as removed.
- Rewrites every affected parquet file without the deleted rows (unaffected files are not touched at all).
- The single commit JSON contains both a `remove` action and an `add` action pointer to the new file.
![[Drawing 2026-06-06 18.33.17.excalidraw]]
# Update
Esentially a `DELETE` + `INSERT` in one atomic commit.
- Delta does not mutate the original Parquet file directly. It <span style="color:rgb(216, 203, 251)">creates a new version of the affected data</span>.
- Uses file-level predicate pushdown. Delta reads the stats in each file's log entry (min/max per column) and <span style="color:rgb(216, 203, 251)">skips files that can't possibly contain matching rows</span>.
- Only affected rows are rewritten.
- If the commit JSON write fails, <span style="color:rgb(216, 203, 251)">the rewritten parquet is orphaned</span>. Never visible to readers.
![[Drawing 2026-06-06 19.07.50.excalidraw]]
# Transaction history
Use `CREATE OR REPLACE` to <span style="color:rgb(216, 203, 251)">retain the transaction history</span> use `DROP` to delete it.