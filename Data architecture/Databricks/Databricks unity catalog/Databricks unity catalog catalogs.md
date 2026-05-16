# List all available catalogs
```sql
SHOW CATALOGS;
```
# See active catalog
```sql
SELECT current_catalog();
```
# Set catalog
```sql
USE CATALOG gizmobox;
```
# Create a catalog
```sql
CREATE CATALOG IF NOT EXISTS gizmobox
  MANAGED LOCATION 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/'
  COMMENT 'This is the catalog for the Gizmobox Data Lakehouse';
```