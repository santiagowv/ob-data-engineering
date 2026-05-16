---
Databricks connections: https://docs.databricks.com/aws/en/connect/
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