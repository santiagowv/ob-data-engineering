Delta Lake supports <span style="color:rgb(216, 203, 251)">different strategies for handling row-level changes</span> such as `DELETE`, `UPDATE`, and `MERGE`.
# Copy-on-write
Delta Lake <span style="color:rgb(216, 203, 251)">rewrites the affected parquet files when rows are updated or deleted</span>. More efficient for read-heavy workloads.
- If a file contains 1 million rows and 10 rows are deleted, <span style="color:rgb(216, 203, 251)">Delta Lake does not modify the file in place</span>.
- It creates a new version of the file without those 10 rows and <span style="color:rgb(216, 203, 251)">marks the old file as removed in the Delta transaction log</span>.
## Key points
- Used when <span style="color:rgb(216, 203, 251)">deletion vectors are disabled</span>.
- Affected parquet <span style="color:rgb(216, 203, 251)">files are rewritten</span>.
- Unaffected files are not touched.
- The delta logs records `remove` actions for old files and `add` actions for new files.
- Reads are simpler because the <span style="color:rgb(216, 203, 251)">active files already represent the latest table state</span>.
- Can be <span style="color:rgb(216, 203, 251)">expensive for frequent small deletes, updates, or merges</span> because entire files may need to be rewritten.
![[Drawing 2026-06-12 19.53.35.excalidraw]]
# Merge-on-read
The orginal parquet files remain unchanged. Instead of rewriting the file immediately, <span style="color:rgb(216, 203, 251)">Delta Lake records deleted rows in a separate deletion vectors</span>. Faster for write-heavy workloads.
- A deletion vector is <span style="color:rgb(216, 203, 251)">metadata that tracks which row positions in a Parquet file</span> should be considered deleted.
- When a query reads the file, <span style="color:rgb(216, 203, 251)">Delta Lake also reads the deletion vector and skips the deleted rows</span> at query time.
## Key points
- Used when deletion vectors are enabled.
- Original parquet <span style="color:rgb(216, 203, 251)">files are not immediately rewritten</span>.
- <span style="color:rgb(216, 203, 251)">Deleted rows are tracked separately</span> using deletion vectors.
- <span style="color:rgb(216, 203, 251)">Queries apply the deletion vector during reads</span> fo filter out deleted rows.
- Especially useful for frequent `DELETE`, UPDATE, and `MERGE` operations.
- Reads may have slightly more overhead because Delta Lake must combine the Parquet file with the deletion vector.
- Over time, maintenance operations such as `OPTIMIZE` or<span style="color:rgb(216, 203, 251)"> file compaction can physically rewrite files and remove the deleted rows permanently</span>.
![[Drawing 2026-06-12 19.47.37.excalidraw]]