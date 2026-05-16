We use volumes when <span style="color:rgb(216, 203, 251)">we need to work with files</span>, not tables, but still want governance.
# Create a volume
```sql
CREATE EXTERNAL VOLUME IF NOT EXISTS operational_data
  LOCATION 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/landing/operational/'
```
# List volume content
```
%fs ls /Volumes/gizmobox/landing/operational_data
```