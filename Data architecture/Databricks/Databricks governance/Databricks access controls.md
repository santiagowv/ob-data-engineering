There are two governance layers:
1. RBAC (Role based access control).
2. ABAC (Attribute based access control).
# RBAC
![[Drawing 2026-03-27 19.33.04.excalidraw]]

# ABAC
<span style="color:rgb(216, 203, 251)">Use govern tags</span> with row level filtering or column masking.
![[Drawing 2026-03-27 19.37.45.excalidraw]]
# Apply govern tags to table columns
```sql
ALTER TABLE abac_demo.customer_data.profiles
	ALTER COLUMN ssn SET TAGS ('pii' = 'ssn');
	
ALTER TABLE abac_demo.customer_data.profiles
	ALTER COLUMN address SET TAGS ('pii' = 'address');
```
# Create UDFs to define govern tags rules
## Row filter
```sql
CREATE OR REPLACE FUNCTION is_not_eu_address(address STRING)
RETURNS BOOLEAN
DETERMINISTIC
RETURN (
	SELECT CASE
		WHEN LOWER(address) LIKE '%eu%'
		  OR LOWER(address) LIKE '%e.u.%'
		  OR LOWER(address) LIKE '%europe%'
		  OR LOWER(address) LIKE '%germany%'
		  OR LOWER(address) LIKE '%france%'
		  OR LOWER(address) LIKE '%spain%'
		  THEN FALSE -- This IS an EU address
		ELSE TRUE    -- This is NOT an EU address
	END
);
```
## Column mask
### Full redaction
```sql
CREATE OR REPLACE FUNCTION mask_ssn_full(ssn STRING)
RETURNS STRING
DETERMINISTIC
RETURN '***-**-****'
```
### Partial
```sql
CREATE OR REPLACE FUNCTION mask_ssn_partial(ssn STRING)
RETURNS STRING
DETERMINISTIC
RETURN CONCAT('***-**', RIGHT(ssn, 4));
```
### Email
Domain only.
```sql
CREATE OR REPLACE FUNCTION mask_email_domain(email STRING)
RETURNS STRING
DETERMINISTIC
RETURN CONCAT('***@', SPLIT(email, '@')[1]);
```
### Phone
```sql
CREATE OR REPLACE FUNCTION mask_phone_hash(phone STRING)
RETURNS STRING
DETERMINISTIC
RETURN CONCAT('REF_', SUBSTR(SHA2(CONCAT(phone, ':v1'), 256), 1, 10));
```
# Create policy
## Row filter policy
```sql
CREATE POLICY hide_eu_customers_policy
ON SCHEMA abac_demo.customers_data
COMMENT 'Filters out EU customer records for US analytics team'
ROW FILTER abac_demo.customer_data.is_not_eu_address
TO 'user@domain.com'
FOR TABLES
MATCH COLUMNS hasTagValue('pii', 'address') AS address_col
USING COLUMNS (address_col);
```
## Column mask policy
```sql
CREATE POLICY mask_ssn_policy
ON SCHEMA abac_demo.customer_data
COMMENT 'Fully masks social security numbers of all users except compliance team'
COLUMN MASK abac_demo.customer_data.mask_ssn_full
TO 'user@domain.com'
FOR TABLES
MATCH COLUMNS hasTagValue('pii', 'ssn') AS ssn_col
ON COLUMN ssn_col;
```
# Drop policy
```sql
DROP POLICY mask_ssn_policy ON SCHEMA abac_demo.customer_data;
```