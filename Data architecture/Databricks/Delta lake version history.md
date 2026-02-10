# Query delta lake table history
```sql
DESCRIBE HISTORY demo.delta_lake.companies;
```
# Query data from a specific version
```sql
SELECT * FROM demo.delta_lake.companies
VERSION AS OF 1;
```
# Query data from a specific time
```sql
SELECT * FROM demo.delta_lake.companies
TIMESTAMP AS OF '2025-01-07T11:45:12.000+00:00';
```
# Restore data in the table to a specific version
```sql
RESTORE TABLE demo.delta_lake.companies VERSION AS OF 1;
```