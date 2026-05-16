# DBFS and Hive Metastore (legacy)
![[Pasted image 20260112102604.png|650]]
## DBFS
Databricks file system for <span style="color:rgb(216, 203, 251)">object storage</span>.
- It can be used with <span style="color:rgb(216, 203, 251)">external object storage services</span> such as S3, GC.
## Hive metastore
Used for <span style="color:rgb(216, 203, 251)">structured data management</span> and <span style="color:rgb(216, 203, 251)">metadata</span> storage.
- Common formats are csv, parquet, avro.
# Unity catalog
It's a <span style="color:rgb(216, 203, 251)">centralized data catalog that provides access control</span>, auditing, quality monitoring, and data discovery capabilities across Databricks workspaces.
![[Pasted image 20260112102711.png|650]]
Volumes are <span style="color:rgb(216, 203, 251)">high-level abstractions of the containers</span> in the external object storage service.
# Unity catalog object model
![[Drawing 2026-01-12 10.34.58.excalidraw|800]]