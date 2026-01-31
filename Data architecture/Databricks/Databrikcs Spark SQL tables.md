# Create external table
The <span style="color:rgb(216, 203, 251)">data doesn't get moved or copied</span>, some metadata gets created in the metastore describing the data and underlying files.
- <span style="color:rgb(216, 203, 251)">Source files are unaffected</span> when the external table gets deleted.
- Can't create a <span style="color:rgb(216, 203, 251)">external table on a volume</span>.
```sql
CREATE TABLE IF NOT EXISTS gizmobox.bronze.payments
(payment_id INTEGER, order_id INTEGER, payment_timestamp TIMESTAMP, payment_status INTEGER, payment_method STRING)
USING CSV
OPTIONS (
  header = "true",
  delimiter = ","
)
LOCATION 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/landing/external/payments'
```
## Refresh external table
<span style="color:rgb(216, 203, 251)">Updates the table's metadata</span> considering any changes in the source files.
```sql
REFRESH TABLE gizmobox.bronze.payments;
```