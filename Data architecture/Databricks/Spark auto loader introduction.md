Auto loader is a new structured <span style="color:rgb(216, 203, 251)">streaming source designed for large-scale, efficient data ingestion</span>.
- Incrementally and efficiently <span style="color:rgb(216, 203, 251)">processes new data files as they arrive in the cloud storage</span>.
- Uses <span style="color:rgb(216, 203, 251)">file notifications</span> for efficient file detection using cloud services.
- <span style="color:rgb(216, 203, 251)">Replaces in-memory tracking</span> with Rocksdb, a distributed key value store, allowing infinite scalability.
- Supports <span style="color:rgb(216, 203, 251)">schema inference from sample files</span>.
- Supports <span style="color:rgb(216, 203, 251)">schema evolution</span>.
# Traditional file stream source limitations
- **Inefficient file using:** <span style="color:rgb(216, 203, 251)">Repeatedly scan</span> entire directories to detect new files.
- **Scalability issues:** All processed <span style="color:rgb(216, 203, 251)">files are stored in an in-memory map</span> on the driver node. There may be memory constraints.
- **Schema evolution problems:** If a new column appears in the incoming data, it may lead to <span style="color:rgb(216, 203, 251)">data loss or requires manual intervention</span>.