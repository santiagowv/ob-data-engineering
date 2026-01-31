# Configure access to cloud storage
1. Create <span style="color:rgb(216, 203, 251)">Databricks Access Connector</span>.
2. Create <span style="color:rgb(216, 203, 251)">Azure Data Lake Storage</span>.
3. Assign Storage <span style="color:rgb(216, 203, 251)">Blob Data Contributor role</span> to the access connector.
4. Create <span style="color:rgb(216, 203, 251)">Storage Credential</span>.
5. Create <span style="color:rgb(216, 203, 251)">External Location</span>.
![[Pasted image 20260112125359.png|650]]
# Create external location
```sql
CREATE EXTERNAL LOCATION IF NOT EXISTS dea_course_extl_dl_gizmobox
	URL 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/'
	WITH (STORAGE CREDENTIAL dea_course_ext_sc)
	COMMENT "External location for the Gizmobox Data Lakehouse"
```