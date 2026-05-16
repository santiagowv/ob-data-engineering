- **Top down permissions:** when we need to <span style="color:rgb(216, 203, 251)">grant bulk permissions to assets in a schema or catalog</span>.
- **Bottom up permissions:** when we need to <span style="color:rgb(216, 203, 251)">grant fine grained permissions</span>.
![[Pasted image 20260328081423.png|611]]
- Only owners can manage permissions on an object.
- Metastore admin can all change permission on objects.
- Metastore admin can also change ownership of objects.
# Grant priviliges
- `<privilige-type>`: is a Unity Catalog <span style="color:rgb(216, 203, 251)">privilige type</span>.
- `<securable-type>`: the <span style="color:rgb(216, 203, 251)">type of securable object</span>, such as `CATALOG` or `TABLE`.
- `<securable-name>`: the <span style="color:rgb(216, 203, 251)">name of the securable</span>. If the securable type is `METASTORE`, do not provide the securable name. It is assumed to be the metastore attached to the workspace.
- `<principal>`: is a user, service principal, or group.
# Check permissions on an object
```sql
SHOW GRANTS ON METASTORE;
```
# Metastore
- `CREATE CATALOG`.
- `CREATE CLEAN ROOM`.
- `CREATE CONNECTION`.
- `CREATE EXTERNAL LOCATION`.
- `CREATE EXTERNAL METADATA`.
- `CREATE PROVIDER`.
- `CREATE RECIPIENT`.
- `CREATE SHARE`.
- `CREATE SERVICE CREDENTIAL`.
- `CREATE STOREAGE CREDENTIAL`.
- `SET SHARE PERMISSION`.
- `USE MARKETPLACE ASSETS`.
- `USE PROVIDER`.
- `USE RECIPIENT`.
- `USE SHARE`.
# Catalog
- `ALL PRIVILIGES`.
- `APPLY TAG`.
- `BROWSE`.
- `CREATE SCHEMA`.
- `USE CATALOG`.
- `CREATE FUNCTION`.
- `CREATE TABLE`.
- `CREATE MATERIALIZED VIEW`.
- `CREATE MODEL`.
- `CREATE VOLUME`.
- `EXTERNAL USE SCHEMA`.
- `READ VOLUME`.
- `REFRESH`.
- `WRITE VOLUME`.
- `EXECUTE`.
- `MANAGE`.
- `MODIFY`.
- `SELECT`.
- `USE SCHEMA`.
# Schema
- `ALL PRIVILEGES`.
- `APPLY TAG`.
- `CREATE FUNCTION`.
- `CREATE TABLE`.
- `CREATE MODEL`.
- `CREATE VOLUME`.
- `CREATE MATERIALIZED VIEW`.
- `MANAGE`.
- `EXTERNAL USE SCHEMA`.
- `USE SCHEMA`.
- `EXECUTE`.
- `MODIFY`.
- `READ VOLUME`.
- `REFRESH`.
- `SELECT`.
- `WRITE VOLUME`.
# Table
- `ALL PRIVILEGES`.
- `APPLY TAG`.
- `MANAGE`.
- `MODIFY`.
- `SELECT`.