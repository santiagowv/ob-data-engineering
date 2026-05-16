There are two governance layers:
1. RBAC (Role based access control).
2. ABAC (Attribute based access control).
# RBAC
![[Drawing 2026-03-27 19.33.04.excalidraw]]

# ABAC
<span style="color:rgb(216, 203, 251)">Use govern tags</span> with row level filtering or column masking. <span style="color:rgb(216, 203, 251)">We must grant table access separately</span> through object-level permission (`GRANT`).
![[Drawing 2026-03-27 19.37.45.excalidraw]]
# Who can manage priviliges?
Priviliges can be granted by any of the following:
- The <span style="color:rgb(216, 203, 251)">owner of the object</span>.
- The <span style="color:rgb(216, 203, 251)">owner of the catalog or schema</span> that contains the object.
- A user with the `MANAGE` privilige on the object.
- A metastore admin.

# Apply govern tags to table columns
```sql
ALTER TABLE abac_demo.customer_data.profiles
	ALTER COLUMN ssn SET TAGS ('pii' = 'ssn');
	
ALTER TABLE abac_demo.customer_data.profiles
	ALTER COLUMN address SET TAGS ('pii' = 'address');
```
# Create UDFs to define govern tags rules
- Python <span style="color:rgb(216, 203, 251)">UDFs are typically less performant than SQL</span>, and they offer fewer opportunities for optimization.
- We <span style="color:rgb(216, 203, 251)">can't apply row-level security or column masks</span> to a view.
## Row filters
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
## Column masks
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
# Policies
Policies are <span style="color:rgb(216, 203, 251)">attached at a level in the Unity Catalog hierarchy, such as catalog, schema, or table</span>, and are evaluated dynamically.
- A single policy can enforce <span style="color:rgb(216, 203, 251)">consistent access rules across an entire catalog or schema</span>.
- A <span style="color:rgb(216, 203, 251)">policy defined at the catalog level applies to all tables in that catalog</span> before the query reaches the runtime.
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
### Variant-based masking
When we need to mask columns of different types (for example, `INT`, `DOUBLE`, `DECIMAL(10, 2)`, `DECIMAL(15, 5)`) <span style="color:rgb(216, 203, 251)">we can write a single masking UDF that accepts and returns a</span> `VARIANT` <span style="color:rgb(216, 203, 251)">type</span>.
### Hashing for deterministic pseudonymization
Replaces sensitive data with a <span style="color:rgb(216, 203, 251)">hashed value that is the same across multiple tables</span>.
- Uses `version` parameter to support key rotation.
## Drop policy
```sql
DROP POLICY mask_ssn_policy ON SCHEMA abac_demo.customer_data;
```
## Policy quotas

| Resource                                                          | Limit  |
| ----------------------------------------------------------------- | ------ |
| Policies per metastore                                            | 10.000 |
| Policies per catalog or schema                                    | 100    |
| Policies per table                                                | 50     |
| Principals per policy (applies to both `TO` and `EXCEPT` clauses) | 20     |
| Column conditions per `MATCH COLUMNS` clause                      | 3      |
## Policies best practices
- Use a <span style="color:rgb(216, 203, 251)">single</span> `sensitivity` <span style="color:rgb(216, 203, 251)">tag with controlled values</span> (`public`, `internal`, `confidential`, `restricted`) rather than multiple overlapping tags like `is_sensitive`, `data_class`, and `pii_level`.
- <span style="color:rgb(216, 203, 251)">Restrict tag creation and modification</span> to authorized data stewards or governance admins.
- Apply a default restrictire tag (like `classification : unverified`) <span style="color:rgb(216, 203, 251)">to new objects until a data steward reviews them</span>.
- Attach <span style="color:rgb(216, 203, 251)">policies at the highest level</span> when possible (catalog or schema).
- <span style="color:rgb(216, 203, 251)">Review policies periodically</span> and consolidate overlapping ones.
- Use `SHOW EFFECTIVE POLICIES` to determine <span style="color:rgb(216, 203, 251)">what applies to a specific table</span>.
- <span style="color:rgb(216, 203, 251)">Document tagging taxonomy, policies, and group management</span> approach so that teams can understand governance model.
# ABAC vs table-level row filters and column masks
Use ABAC policies when:
- Need <span style="color:rgb(216, 203, 251)">consistent access rules</span> across many tables, schemas, or catalogs.
- Data estate is growing and <span style="color:rgb(216, 203, 251)">want new tables convered automatically</span> when they are tagged.
- Allow operations like: time travel, delta sharing, full query optimization.
Use table-level row filters and column masks when:
- Each <span style="color:rgb(216, 203, 251)">table has specific logic that doesn't generalize</span> to other tables.
- Small stable <span style="color:rgb(216, 203, 251)">set of tables that change infrequently</span>.
# Dynamic views

| Feature       | Applies to     | Managed using          | Naming impact             | Best used for                                                                                    |
| ------------- | -------------- | ---------------------- | ------------------------- | ------------------------------------------------------------------------------------------------ |
| Dynamic views | Views          | SQL logic              | Creates a new object name | <span style="color:rgb(216, 203, 251)">Sharing filtered data</span> or spanning multiple tables  |
| Row filters   | Tables         | ABAC or mapping tables | Table name unchaged       | Row-level access <span style="color:rgb(216, 203, 251)">control tied to user or data tags</span> |
| Column masks  | Tables/columns | ABAC or mapping tables | Table name unchaged       | Redacting <span style="color:rgb(216, 203, 251)">sensitive column data based on identity</span>  |

- Use dynamic views when we need <span style="color:rgb(216, 203, 251)">fine-grained access control that spans multiple source tables</span> or reshapes data for sharing. 
- Use row filters and column masks when we want to <span style="color:rgb(216, 203, 251)">control access on individual tables without introducting new objects</span>.