# Working with JSON values as text
## Extract top level object values
Top level object `<column_name>:<extraction_path>`.
```sql
SELECT 
value:order_id AS order_id,
value
FROM gizmobox.bronze.v_orders;
```
## Extract array elements
To extract array elements - <span style="color:rgb(216, 203, 251)">use the indices in brackets</span> `<column_name>:<extraction_path>[index]`.
```sql
SELECT
value:items[0] AS item_1,
value:items[1] AS item_2,
value
FROM gizmobox.bronze.v_orders;
```
## Extract nested column values
To extract nested fields, <span style="color:rgb(216, 203, 251)">use dot notation</span> `<column_name>:<extraction_path>[index].<leaf node>`.
```sql
SELECT
	value:items[0].item_id AS item_1_item_id,
	value:items[0] AS item_1,
	value:items[1] AS item_2,
	value
FROM gizmobox.bronze.v_orders;
```
## CAST column values to a specific data type
```sql
SELECT 
	value:items[0].item_id::INTEGER AS item_1_item_id,
	value:items[0] AS item_1,
	value:items[1] AS item_2,
	value
FROM gizmobox.bronce.v_orders;#
```
# Working with JSON values as objects
## Get JSON object schema
```sql
SELECT
schema_of_json(fixed_value) AS schema,
fixed_value
FROM tv_orders_fixed
LIMIT 1;
```
## Convert text to JSON object
```sql
SELECT
from_json(fixed_value,
          'STRUCT<customer_id: BIGINT, items: ARRAY<STRUCT<category: STRING, details: STRUCT<brand: STRING, color: STRING>, item_id: BIGINT, name: STRING, price: BIGINT, quantity: BIGINT>>, order_date: STRING, order_id: BIGINT, order_status: STRING, payment_method: STRING, total_amount: BIGINT, transaction_timestamp: STRING>') AS json_value

FROM tv_orders_fixed;
```
## Access element from JSON object
`<column_name.object>`
```sql
SELECT
json_balue.order_id
FROM gizmobox.silver.orders_json;
```
## Deduplicate array elements
```sql
SELECT
array_distinct(json_value.items) as items
FROM gizmobox.silver.orders_json;
```
## Explode arrays
```sql
SELECT
explode(array_distinct(json_value.items)) as items
FROM gizmobox.silver.orders_json;
```