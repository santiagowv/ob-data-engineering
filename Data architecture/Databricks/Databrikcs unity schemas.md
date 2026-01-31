# List all available schemas
```sql
SHOW SCHEMAS;
```
# See active schema
```sql
SELECT current_schema();
```
# Set schema
```sql
USE SCHEMA landing;
```
# Create a schema
```sql
CREATE SCHEMA IF NOT EXISTS landing
  COMMENT 'This is the landing zone for the Gizmobox Data Lakehouse'
  MANAGED LOCATION 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/landing';
```