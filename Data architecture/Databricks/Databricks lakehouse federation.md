<span style="color:rgb(216, 203, 251)">Access and govern data stored outside of the Lakehouse</span> without physically moving or copying it into the Lakehouse.
![[Pasted image 20260113132602.png|650]]
Best practice in real projects:
- If <span style="color:rgb(216, 203, 251)">more than one team uses the same external data</span> use Catalog Federation.
- If it's <span style="color:rgb(216, 203, 251)">temporary, exploratory, or low-volume</span> use Query Federation.
- If <span style="color:rgb(216, 203, 251)">performance becomes critical</span>, ingest into Delta Lake.
# Supported sources
## Databases and data warehouses
- MySQL.
- PostgreSQL.
- Teradata.
- Oracle.
- Amazon Redshift.
- Salesforce Data Cloud.
- Snowflake.
- Microsfot SQL Server.
- Azure Synapse.
- Google BigQuery.
## Other catalogs
- Legacy Databricks Hive.
- Metastore.
- External Hive.
- AWS Glue metastore.
- Salesfor Data Cloud.
- Snowflake.
# Federation types
## Query federation
- <span style="color:rgb(216, 203, 251)">Direct JDBC connection</span> to external databases (eg. MySQL, SQL Server, Redshift).
- Query executed on the external database.
- Performance/cost depend on source system.
- Can <span style="color:rgb(216, 203, 251)">join with Lakehouse data</span>.
- Best for <span style="color:rgb(216, 203, 251)">ad-hoc or PoC</span>.
### Query federation implementation
1. Unity catalog enabled.
2. Create connection (JDBC + credentials).
3. Create foreign catalog.
4. Grant privileges.
5. Run queries -> pushed down to external DB.
```sql
CREATE CONNECTION asql_gizmobox_db_conn_sql TYPE sqlserver
OPTIONS (
  host 'gizmobox-srv.database.windows.net',
  port '1433',
  user 'gizmoboxadm',
  password 'Gizmobox@Adm'
);
```
Create foreign catalog:
```sql
CREATE FOREIGN CATALOG IF NOT EXISTS asql_gizmobox_db_catalog_sql
USING CONNECTION asql_gizmobox_db_conn_sql
OPTIONS (database 'gizmobox-db');
```
## Catalog federation
- Connects to external cataglos (AWS Glue, Hive, Metastore, Snowflake).
- Query <span style="color:rgb(216, 203, 251)">executed on Databricks compute</span>.
- More <span style="color:rgb(216, 203, 251)">cost-effective and performance optimized</span>.
- Can join with Lakehouse data.
- Best for migration or hybrid models.
### Catalog federation implementation
1. Unity catalog enabled.
2. Create connection to external catalog.
3. Create storage credential + external location.
4. Create foreign catalog.
5. Grant privileges.
6. Run queries -> executed on object storage.
### Query a table
```sql
SELECT r.*
FROM asql_gizmobox_db_catalog_sql.dbo.refunds r
```