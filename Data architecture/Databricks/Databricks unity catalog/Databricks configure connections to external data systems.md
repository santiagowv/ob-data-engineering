---
Databricks connections: https://docs.databricks.com/aws/en/connect/
Databricks SFTP connector: https://docs.databricks.com/aws/en/ingestion/sftp
---
# Query federation connectors
Provides <span style="color:rgb(216, 203, 251)">read-only access to data</span> in enterprise data systems.
- <span style="color:rgb(216, 203, 251)">Uses secure JDBC connection</span> to federate to external data systems.
- [[Databricks lakehouse federation]] connects external catalogs, such as Hive metastore, AWS GLue, or Snowflake Horizon Catalog.
# Managed ingestion connectors
Allows admin users to <span style="color:rgb(216, 203, 251)">create a connection and a managed ingestion pipeline</span> at the same time in the data ingestion UI.
# Streaming connectors
Provides <span style="color:rgb(216, 203, 251)">optimized connectors for many streaming data systems</span>.
- Databricks recommends storing credentials using secrets.
- Common connectors:
	- Cloud object storage.
	- SFTP servers.
	- Apache kafka.
	- Amazon kinesis.
	- Google Pub/Sub.
	- Apache Pulsar.
# Thid-party integrations
Use third-party tools to <span style="color:rgb(216, 203, 251)">connect to external data sources and automate ingesting data to the lakehouse</span>.
# Drivers
Databricks includes drivers for external data systems in each Databricks Runtime. We can <span style="color:rgb(216, 203, 251)">optionally install third-party drivers to access data in other systems</span>.
- For read-only query federation, Lakehouse Federation is always preferred over these drivers.
# JDBC
Connect to external databases using <span style="color:rgb(216, 203, 251)">JDBC with a Unity Catalog connection for governed access</span>.
# Unity catalog credential vending
Grants <span style="color:rgb(216, 203, 251)">short-lived credentials using the Unity Catalog REST API</span>. The granted credentials inherit the priviliges of the Databricks principal used to configure the integration.
- **Table credential vending:** provides <span style="color:rgb(216, 203, 251)">access to data registered</span> in Unity Catalog metastore.
- **Path credential vending:** provides <span style="color:rgb(216, 203, 251)">access to external locations</span> in Unity Catalog metastore.
- **Volume credential vending:** lets <span style="color:rgb(216, 203, 251)">external engines access files stored in Unity Catalog volumes</span> with temporary, scoped credentials.
## Requirements
- <span style="color:rgb(216, 203, 251)">External access must be configured on the metastore</span> with `EXTERNAL USE SCHEMA` granted to the requesting principal.
- <span style="color:rgb(216, 203, 251)">Workspace URL must be accessible to the requesting engine</span>, including engines behind IP access lists.
# Ingest files from SFTP servers
SFTP connector extends auto loader functionality to provide secure, <span style="color:rgb(216, 203, 251)">incremental ingestion from SFTP servers with Unity Catalog Governance</span>.
The SFTP connector offers the following:
- <span style="color:rgb(216, 203, 251)">Private key and password-based</span> authentication.
- <span style="color:rgb(216, 203, 251)">Incremental file ingestion and processing</span> with exactly-once guarantees.
- Automatic schema inference, evolution, and data rescue.
- Unity catalog <span style="color:rgb(216, 203, 251)">governance for secure ingestion and credentials</span>.
- Wide file format support:
	- `JSON,` `CSV`, `XML`, `PARQUET`, `AVRO`, `TEXT`, `BINARYFILE`, and `ORC`.
- <span style="color:rgb(216, 203, 251)">Built-in support for pattern and wildcard matching</span> to easily target data subsets.
## Read files from the SFTP server with autoloader
```python
# Run the Auto Loader job to ingest all existing data in the SFTP server.  
# The <username> and <host> in the URI must match the connection created in the previous step.  
# The connector automatically resolves the matching Unity Catalog connection for authentication.  
df = (spark.readStream.format("cloudFiles")  
.option("cloudFiles.schemaLocation", "<path to store schema information>") # This is a cloud storage path  
.option("cloudFiles.format", "csv") # Or other format supported by Auto Loader  
# Specify the absolute path on the SFTP server starting from the root /.  
# Example: /home/<username>/data/files or /uploads/csv_files  
.load("sftp://<username>@<host>:<port>/<absolute_path_to_files>")  
.writeStream  
.format("delta")  
.option("checkpointLocation", "<path to store checkpoint information>") # This is a cloud storage path.  
.trigger(availableNow = True)  
.table("<table name>"))  
df.awaitTermination()
```
### Auto loader lakeflow spark declarative pipelines
```sql
CREATE OR REFRESH STREAMING TABLE sftp_bronze_table  
AS SELECT * FROM STREAM read_files(  
"sftp://<username>@<host>:<port>/<absolute_path_to_files>",  
format => "csv"  
)
```
## Limitations
- SFTP is not supported across other ingestion surfaces, including `COPY INTO`, `spark.read` and `dbutils.ls`.