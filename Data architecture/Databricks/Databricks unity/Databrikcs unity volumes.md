We use volumes when you need to work with files, not tables, but still want governance.
# Create a volume
```sql
CREATE EXTERNAL VOLUME IF NOT EXISTS operational_data
  LOCATION 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/landing/operational/'
```
# List volume content
```
%fs ls /Volumes/gizmobox/landing/operational_data
```