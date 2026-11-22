![[Pasted image 20260525102154.png|487]]
An object in Snowflake is <span style="color:rgb(216, 203, 251)">something we can interact with or issue commands against</span>.
- Database.
- Table.
- Warehouse.
# Object hierarchy
![[Pasted image 20260525112645.png|598]]
## Organization
Special type of <span style="color:rgb(216, 203, 251)">administrative Snowflake account</span> that allows a busines to centrally <span style="color:rgb(216, 203, 251)">manage and oversee all the standard Snowflake accounts it owns</span>.
- Manage standard accounts.
- Monitor standard accounts.
- Enable cross-account features.
## Standard account
A <span style="color:rgb(216, 203, 251)">private, isolated Snowflake environment</span> for our own use, accessible through a <span style="color:rgb(216, 203, 251)">unique URL</span> that combines our organization and account identifiers.
- Each Snowflake account <span style="color:rgb(216, 203, 251)">runs on a sinble cloud provider</span>.
- Each Snowflake account <span style="color:rgb(216, 203, 251)">exists in a single geographic region</span>.
- Each Snowflake account is a <span style="color:rgb(216, 203, 251)">single edition</span>, i.e. enterprise.
- Each Snowflake account is created with <span style="color:rgb(216, 203, 251)">several system roles</span>.
## Account level objects
- Network policies.
- User.
- Role.
- Database.
- Warehouse.
- Share.
- Resource monitor.
- Schema.
## Database objects
- Stage.
- Pipe.
- Procedure.
- Table.
- Function.
- View.
- Task.
# Database
Must have a <span style="color:rgb(216, 203, 251)">unique identifier in an account</span>.
- Must start with an <span style="color:rgb(216, 203, 251)">alphabetic character and cannot contain spaces or special characters</span> unless enclosed in double quotes.
```sql
CREATE DATABASE my_database;
```

```sql
CREATE DATABASE my_db_clone CLONE mytestdb;
```

```sql
CREATE DATABASE mydb1
	AS REPLICA OF myorg.account1.mydb1
	DATA_RETENTION_TIME_IN_DAYS = 10;
```

```sql
CREATE DATABASE shared_db FROM SHARE utt783.share;
```
# Schema
Schemas <span style="color:rgb(216, 203, 251)">must have a unique identifier</span> in a database.
- Must <span style="color:rgb(216, 203, 251)">start with an alphabetic character and cannot contain spaces or special characters</span> unless enclosed in double quotes.
```sql
CREATE SCHEMA my_schema;
```

```sql
CREATE SCHEMA my_schema_clone CLONE my_schema;
```
# Table types
## Permanent
- Default table type.
- Permanent tables are created with the `CREATE TABLE <tablename>` syntax.
- Permanent tables can have a maximum <span style="color:rgb(216, 203, 251)">data retention period of 90 days with the enterprise edition and higher</span>. For the standard edition this will be a maximum of 1 day.
- Support <span style="color:rgb(216, 203, 251)">fail-safe retetion period of 7 days</span>.
- Exists until explicitly removed.
- Best suited for tables that need to retain data permanently like <span style="color:rgb(216, 203, 251)">FACTS and DIMENSIONS</span>.
## Temporary
- Temporary tables are created with the Keyword `TEMPORARY`.
- Used for transitory data.
- <span style="color:rgb(216, 203, 251)">Persist for duration of a session</span>. Once the session ends the temporary tables are dropped and all data is permanently deleted.
- <span style="color:rgb(216, 203, 251)">Best suited to temporarily store data that is required within a session like a stored procedure</span> execution to store data for intermediate calculation.
- Time travel 1 day.
- No fail-safe.
## Transient
- Transient tables are created with the keyword `TRANSIENT`.
- <span style="color:rgb(216, 203, 251)">Maximum of 1 day of travel time</span> irrespective of the Snowflake edition.
- Used in cases where <span style="color:rgb(216, 203, 251)">data in tables are stored temporarily and deleted within a few days</span>.
- Exists until explicitly dropped.
- No fail-safe period.
## External
- Query data outside Snowflake.
- Read-only table.
- No time travel.
- No fail-safe.
## Hybrid
- Supports OLTP and OLAP.
- Stores data in a row-based format.
- Enforces referential integrity constraints.
## Iceberg
- Apache Iceberg table format.
- Manage data from within Snowflake.
- Does not support time-travel and fail-safe.
# View types
## Standard
```sql
CREATE VIEW my_view AS
SELECT col1, col2 FROM my_table;
```
- Does <span style="color:rgb(216, 203, 251)">not contribute to storage cost</span>.
- If source table is removed, <span style="color:rgb(216, 203, 251)">querying view returns error</span>.
- Used to <span style="color:rgb(216, 203, 251)">restrict contents of a table</span>.
## Materialized
```sql
CREATE MATERIALIZED VIEW my_view AS
SELECT col1, col2 FROM my_table;
```
- Stores results of a <span style="color:rgb(216, 203, 251)">select statement definition</span> and <span style="color:rgb(216, 203, 251)">periodically refreshes it</span>.
- <span style="color:rgb(216, 203, 251)">Incurs costs</span> as a serverless feature.
- Used to <span style="color:rgb(216, 203, 251)">boost performance of external tables</span>.
## Secure
```sql
CREATE SECURE VIEW my_view AS
SELECT col1, col2 FROM my_table;
```
- Both <span style="color:rgb(216, 203, 251)">standard and materialized can be secure</span>.
- Uderlying query definition <span style="color:rgb(216, 203, 251)">only visible to authorized users</span>.
- Some <span style="color:rgb(216, 203, 251)">query optimizations bypassed</span> to improve security.
# Data types
- **Numeric:** INTEGER.
- **String:** VARCHAR.
- **Logical:** BOOLEAN.
- **Date and time:** DATE.
- **Semi-structured:** ARRAY.
- **Geospatial:** GEOGRAPHY.