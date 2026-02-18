![[Pasted image 20260208100110.png]]
It's the optimized storage layer that <span style="color:rgb(216, 203, 251)">provides the foundation for tables in a Lakehouse on Databricks</span>.
- The storage layer of a data lake house in <span style="color:rgb(216, 203, 251)">Databricks is Delta Lake by default</span>.
# ACID transactions
Open source software that extends Parquet data files with a file-based transaction log for <span style="color:rgb(216, 203, 251)">ACID transactions</span> and <span style="color:rgb(216, 203, 251)">scalable metadata handling</span>.
- Concurrent transactions can be carried out <span style="color:rgb(216, 203, 251)">without affecting the integrity</span>.
- <span style="color:rgb(216, 203, 251)">Manages rollback of partially commited transactions</span> in the event of process failures.
# Scalable metadata
Stores metadata in <span style="color:rgb(216, 203, 251)">JSON files</span>.
# Time travel
With the help of transaction logs <span style="color:rgb(216, 203, 251)">query previous versions of a table</span>.
# Simple solution architecture
It allows us to use a single copy of data for <span style="color:rgb(216, 203, 251)">both batch and streaming operations</span> and providing <span style="color:rgb(216, 203, 251)">incremental processing</span> at scale.
# Support for DML operations
Supports all DML <span style="color:rgb(216, 203, 251)">operations such as insert, update, delete</span>.