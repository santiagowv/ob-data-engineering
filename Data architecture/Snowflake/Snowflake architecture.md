![[Pasted image 20260525091620.png]]
- <span style="color:rgb(216, 203, 251)">Decouple</span> storage, compute and management services.
- Three infinitely <span style="color:rgb(216, 203, 251)">scalable layers</span>.
- Workload <span style="color:rgb(216, 203, 251)">isolation with virtual warehouses</span>.
# Storage layer
- Snowflake <span style="color:rgb(216, 203, 251)">manages all aspects of how data is stored</span> including the distribution compression and metadata storage.
- Data inside tables in Snowflake are stored in <span style="color:rgb(216, 203, 251)">micro-partitions</span>.
- Snowflake tables are atuomatically divided into micro-partitions each of which contains <span style="color:rgb(216, 203, 251)">between 50 MB and 500 MB of uncompressed data</span>.
# Query processing layer
- Virtual warehouses is the <span style="color:rgb(216, 203, 251)">processor used to run SQL queries operations</span>.
- Snowflake <span style="color:rgb(216, 203, 251)">relies on AWS EC2 instances to create virtual warehouses</span> and provide compute for query processing.
- The process of provisioning an EC2 instance is completely <span style="color:rgb(216, 203, 251)">managed by Snowflake</span>.
# Cloud services layer
- <span style="color:rgb(216, 203, 251)">Collection of services that coordinate activities</span> across snowflake database storage and query processing layer.
- The services in this layer include:
	- Authentication and access control.
	- Infrastructure manager.
	- Query parsing and optimization.
	- Results cache.
	- Metadata manager.
	- Security.
## Infrastructure manager
- <span style="color:rgb(216, 203, 251)">Provisioning and scaling of virtual warehouses</span> is done by the infrastructure manager.
- <span style="color:rgb(216, 203, 251)">Optimizes the use of cloud resources</span> ensuring that they are used efficiently and cost-effectively. This includes optimizing network configurations and storage utilization.
- <span style="color:rgb(216, 203, 251)">Provisioning networking components</span> needed to communicate with cloud service providers.
## Query parsing and optimization
1. <span style="color:rgb(216, 203, 251)">Syntax checking</span> is done to verify correctness of SQL syntax.
2. <span style="color:rgb(216, 203, 251)">Execution plan</span> to run the SQL is generated.
3. <span style="color:rgb(216, 203, 251)">Resource allocation</span> to run the query is provided.
4. The execution plan is passed on to the <span style="color:rgb(216, 203, 251)">query processing layer for execution</span>.
## Results cache
- <span style="color:rgb(216, 203, 251)">Stores the results of queries</span> that have been executed previously.
- Result cache is <span style="color:rgb(216, 203, 251)">reused if an identical query is submitted</span> again and the underlying data hasn't changed.
- Cached results are <span style="color:rgb(216, 203, 251)">stored for 24 hours</span>.
- Fetching data from the cache <span style="color:rgb(216, 203, 251)">saves credits and is much faster</span>.
- Even the <span style="color:rgb(216, 203, 251)">order of the columns and the case of the query must not change</span>.
## Metadata manager
- <span style="color:rgb(216, 203, 251)">Stores and manages metadata information</span> like table names and their associated column names inside the metadata cache. It stores:
	- Table statistics.
	- Row count.
	- Object dependencies.
- Metadata manager and cache <span style="color:rgb(216, 203, 251)">help speed up query parsing</span> by providing information on table and column names.
## Security
