---
Constraints on Databricks: https://docs.databricks.com/aws/en/tables/constraints
---

- Enforced constraints <span style="color:rgb(216, 203, 251)">verify data integrity before adding rows to a table</span>.
- Informational primary key and foreign key <span style="color:rgb(216, 203, 251)">constraints define relationships between fields in tables and aren't enforced</span>.
# Enforced constraints on Databricks
When a <span style="color:rgb(216, 203, 251)">constraint is violated, the transaction fails with an error</span>.
- `NOT NULL`: indicates that <span style="color:rgb(216, 203, 251)">values in specific columns cannot be null</span>.
- `CHECK`: indicates that a <span style="color:rgb(216, 203, 251)">specified boolean expression must be true for each input row</span>.
## Set a NOT NULL constraint in Databricks
Databricks <span style="color:rgb(216, 203, 251)">verifies that all existing rows satisfy the constraint</span> before adding a `NOT NULL` constraint.
```sql
CREATE TABLE people10m (
	id INT NOT NULL,
	firstName STRING,
	middleName STRING NOT NULL,
	lastName STRING,
	gender STRING,
	birthDate TIMESTAMP,
	ssn STRING,
	salary INT
);

ALTER TABLE people10m ALTER COLUMN middleName DROP NOT NULL;  
ALTER TABLE people10m ALTER COLUMN ssn SET NOT NULL;
```
## Set a check constraint in Databricks
It <span style="color:rgb(216, 203, 251)">doesn't accept the following types</span> of functions:
- User-defined functions.
- Aggregate functions.
- Window functions.
- Functions returning multiple rows.
```sql
CREATE TABLE people10m (  
id INT,  
firstName STRING,  
middleName STRING,  
lastName STRING,  
gender STRING,  
birthDate TIMESTAMP,  
ssn STRING,  
salary INT  
);  
  
ALTER TABLE people10m ADD CONSTRAINT dateWithinRange CHECK (birthDate > '1900-01-01');  
ALTER TABLE people10m DROP CONSTRAINT dateWithinRange;
```
# Declare primary key and foreign key relationships
Primary and foreign key are <span style="color:rgb(216, 203, 251)">informational only and aren't enforced</span>.
- Foreign keys <span style="color:rgb(216, 203, 251)">must reference a primary key in another table</span>.
- Informational key constraints <span style="color:rgb(216, 203, 251)">might improve performance with query optimizations</span>.
```sql
CREATE TABLE T(pk1 INTEGER NOT NULL, pk2 INTEGER NOT NULL,  
CONSTRAINT t_pk PRIMARY KEY(pk1, pk2));  
CREATE TABLE S(pk INTEGER NOT NULL PRIMARY KEY,  
fk1 INTEGER, fk2 INTEGER,  
CONSTRAINT s_t_fk FOREIGN KEY(fk1, fk2) REFERENCES T);
```
## Add to existing tables
```sql
ALTER TABLE T ADD CONSTRAINT t_pk PRIMARY KEY(pk1, pk2);  
ALTER TABLE S ADD CONSTRAINT s_t_fk FOREIGN KEY(fk1, fk2) REFERENCES T;
```