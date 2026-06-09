Each successful transaction creates a new table version.
# Query delta lake table history
View the transaction history of a Delta table.
```sql
DESCRIBE HISTORY demo.delta_lake.companies;
```
# Query data from a specific version
Query a previous version of a Delta table.
```sql
SELECT * FROM demo.delta_lake.companies
VERSION AS OF 1;
```
# Query data from a specific timestamp
Query a Delta table as it existed at a specific point in time.
```sql
SELECT * FROM demo.delta_lake.companies
TIMESTAMP AS OF '2025-01-07T11:45:12.000+00:00';
```
# Restore data in the table to a specific version
Restore a Delta table to a previous version using `RESTORE TABLE`
```sql
RESTORE TABLE demo.delta_lake.companies VERSION AS OF 1;
```