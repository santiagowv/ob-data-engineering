# Create a view
When a user queries a view the <span style="color:rgb(216, 203, 251)">SELECT statement that defines it gets executed</span>.
```sql
CREATE OR REPLACE VIEW gizmobox.bronze.v_customers
AS
SELECT
_metadata.file_path AS file_path,
*
FROM json.`/Volumes/gizmobox/landing/operational_data/customers`
```
# Query a view
```sql
SELECT * FROM gizmobox.bronze.v_customers;
```
# Create a temporary view
It won't be a <span style="color:rgb(216, 203, 251)">unity catalog object</span>. 
## Session temporary view
It exists for the <span style="color:rgb(216, 203, 251)">duration of the spark session</span> (notebook attach to a cluster).
```sql
CREATE OR REPLACE TEMPORARY VIEW gtv_customers
AS
SELECT
_metadata.file_path AS file_path,
*
FROM json.`/Volumes/gizmobox/landing/operational_data/customers`
```
## Global temporary view
It exists <span style="color:rgb(216, 203, 251)">until the spark cluster gets terminated</span>.
- It can be <span style="color:rgb(216, 203, 251)">accessed from multiple notebooks</span>.
```sql
CREATE OR REPLACE GLOBAL TEMPORARY VIEW gtv_customers
AS
SELECT
_metadata.file_path AS file_path,
*
FROM json.`/Volumes/gizmobox/landing/operational_data/customers`
```
### Query a global temporary view
```sql
SELECT * FROM global.gtv_customers
```