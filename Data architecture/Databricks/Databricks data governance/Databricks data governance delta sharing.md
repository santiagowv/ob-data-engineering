Protocol for <span style="color:rgb(216, 203, 251)">sharing data with external organizations or different metastores</span>.
# Open sharing
<span style="color:rgb(216, 203, 251)">Share data with any user</span>, whether or not they have access to Azure Databricks.
# Databricks-to-Databricks
<span style="color:rgb(216, 203, 251)">Share data with Azure Databricks users</span> whose workspace is attached to a Unity Catalog metastore.
# Configure delta sharing
## Provider
1. **Enable delta sharing on the Metastore:** skip this if sharing within the same Databricks account.
2. **Grant privileges to users who will manage shares:**
```sql
GRANT CREATE SHARE, CREATE RECIPIENT ON METASTORE TO 'data-steward@company.com';
```
3. **Create a share:**
```sql
CREATE SHARE IF NOT EXISTS my_share
COMMENT 'Sharing sales data with partner.'
```
4. **Add data assets to the share:**
```sql
-- add a table
ALTER SHARE my_share ADD TABLE catalog.schema.my_table;

-- add with history
ALTER SHARE my_share ADD TABLE catalog.schema.my_table WITH HISTORY;

-- add an entire schema
ALTER SHARE my_share ADD SCHEMA catalog.my_schema;
```
5. **Create a recipient**:
	- For databricks-to-databricks.
```sql
CREATE RECIPIENT my_partner_recipient
USING ID 'aws:us-east-1:abc123-sharing-identifier';
```
	- For open sharing:
```sql
CREATE RECIPIENT external_recipient;
-- Then download the credential file and send it securely.
```
6. **Grant the recipient access to the share:**
```sql
GRANT SELECT ON SHARE my_share TO RECIPIENT my_partner_recipient;
```
## Recipient
The user must <span style="color:rgb(216, 203, 251)">first create a catalog from the share</span>. 