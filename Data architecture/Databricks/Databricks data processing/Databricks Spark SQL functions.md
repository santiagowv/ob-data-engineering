# COUNT_IF
Counts values in a column that <span style="color:rgb(216, 203, 251)">match a condition</span>.
```sql
SELECT
COUNT(*),
COUNT_IF(customer_id IS NULL),
COUNT_IF(email IS NULL),
COUNT_IF(telephone IS NULL)
FROM
gizmobox.bronze.v_customers;
```
# DATE_FORMAT
<span style="color:rgb(216, 203, 251)">Converts a timestamp to a string</span> the format `fmt`.
```sql
SELECT
	payment_id,
	order_id,
	CAST(date_format(payment_timestamp, 'yyyy-MM-dd') AS date) AS payment_date,
	CAST(date_format(payment_timestamp, 'HH:mm:ss') AS timestamp) AS payment_time,
	payment_status,
	payment_method
FROM gizmobox.bronze.payments;
```
# REGEX
## REGEXP_EXTRACT
```sql
SELECT
refund_id,
payment_id,
regexp_extract(refund_reason, '^([^:]+):', 1) AS refund_reason,
regexp_extract(refund_reason, '^[^:]+:(.*)$', 1) AS refund_source
FROM asql_gizmobox_db_catalog_ui.dbo.refunds;
```
## REGEXP_REPLACE
```sql
SELECT
value,
regexp_replace(value, '"order_date": (\\d{4}-\\d{2}-\\d{2})', '"order_date": "\$1"') AS fixed_value
FROM gizmobox.bronze.v_orders;
```
# PIVOT
Transforms the <span style="color:rgb(216, 203, 251)">rows of the preceding table_reference by rotating unique values</span> of a specified column list into separate columns.
```sql
CREATE TABLE gizmobox.silver.addresses
SELECT
	*
	FROM (SELECT
		  customer_id,
		  address_type,
		  address_line_1,
		  city,
		  state,
		  postcode
		  FROM gizmobox.bronze.v_addresses)
PIVOT (MAX(address_line_1) AS address_line_1,
	   MAX(city) AS city,
	   MAX(state) AS state,
	   MAX(postcode) AS postcode
	   FOR address_type IN ('shipping', 'billing')); -- columns to pivot by
```