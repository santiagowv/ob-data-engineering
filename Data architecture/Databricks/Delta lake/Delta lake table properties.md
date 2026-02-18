# Comment
<span style="color:rgb(216, 203, 251)">Document</span> the purpose of the table.
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_name STRING,
	 founded_date DATE,
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
```
# TBLPROPERTIES
Specify table level <span style="color:rgb(216, 203, 251)">metadata</span> or configuration <span style="color:rgb(216, 203, 251)">settings</span>.
## Sensitive data property
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_name STRING,
	 founded_date DATE,
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
TBLPROPERTIES ('sensitive' = 'true')
```
# Column properties
## Constraints
### NOT NULL
<span style="color:rgb(216, 203, 251)">Enforces data integrity and quality</span> by ensuring that a specific column cannot contain NULL values.
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_name STRING NOT NULL,
	 founded_date DATE,
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
TBLPROPERTIES ('sensitive' = 'true')
```
## Comment
<span style="color:rgb(216, 203, 251)">Documents</span> the purpose or context of individual columns in a table.
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_name STRING NOT NULL,
	 founded_date DATE COMMENT 'The date the company was founded',
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
TBLPROPERTIES ('sensitive' = 'true')
```
## Partition table
```sql
CREATE TABLE demo.delta_lake.gold_companies_partitioned
	(company_name STRING,
	 founded_date DATE
	 country      STRING
PARTITIONED BY (country);
```
# Generated columns
Derived or computed columns, whose <span style="color:rgb(216, 203, 251)">values are computed at the time of inserting new records</span>.
## Generated identity columns
Used to <span style="color:rgb(216, 203, 251)">generate an identity</span> for example a primary key value.
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_id BIGINT NOT NULL GENERATED ALWAYS AS IDENTITY (START WITH 1 INCREMENT BY 1)
	 company_name STRING NOT NULL,
	 founded_date DATE COMMENT 'The date the company was founded',
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
TBLPROPERTIES ('sensitive' = 'true')
```
## Generated computed columns
`expr` may be <span style="color:rgb(216, 203, 251)">composed of literals, column identifiers within the table, and deterministic</span>, built-in SQL functions or operations except:
- Aggregate functions.
- Analytic window functions.
- Ranking window functions.
- Table valued generator functions.
```sql
DROP TABLE IF EXISTS demo.delta_lake.companies;
CREATE TABLE demo.delta_lake.companies
	(company_id BIGINT NOT NULL GENERATED ALWAYS AS IDENTITY (START WITH 1 INCREMENT BY 1)
	 company_name STRING NOT NULL,
	 founded_date DATE COMMENT 'The date the company was founded',
	 founded_year INT GENERATED ALWAYS AS (YEAR(founded_date)),
	 country STRING)
COMMENT 'This table contains information about some of the successful tech companies.'
TBLPROPERTIES ('sensitive' = 'true')
```
# Alter table
<span style="color:rgb(216, 203, 251)">Update</span> existing table properties.
```sql
ALTER TABLE demo.delta_lake.companies_china
	ALTER COLUMN founded_date COMMENT "Date the company was founded"
	ALTER COLUMN company_id SET NOT NULL;
```