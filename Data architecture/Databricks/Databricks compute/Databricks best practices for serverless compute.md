# Specify Python package versions
<span style="color:rgb(216, 203, 251)">Pin the Python packages to specific versions</span> to ensure reproducible environments. Otherwise, the package may resolve to a different version based on the serverless environment.
# Use unique names for temporary views
Serverless compute uses Spark Connect, a client-server architecture that <span style="color:rgb(216, 203, 251)">evaluates temporary views lazily</span>. 
- It can cause <span style="color:rgb(216, 203, 251)">errors when code reuses the same temporary view name</span>, such as in a loop.
# Networking and connectivity
<span style="color:rgb(216, 203, 251)">Serverless compute does not support VPC peering</span>, which is a common way to connect classic compute to data sources in our cloud account.
## Private endpoints
We can use private endpoints when we require <span style="color:rgb(216, 203, 251)">private connectivity from serverless</span>.
- If private connectivity is not possible or required, <span style="color:rgb(216, 203, 251)">we can still protect our resources by creating a firewall around them</span> that allows Databricks serverless traffic.
# Serverless environment versions
We write code against an environment version, and <span style="color:rgb(216, 203, 251)">Databricks independently upgrades the underlying server</span>.
# Choose a performance mode
<span style="color:rgb(216, 203, 251)">Databricks serverless compute offers two performance modes</span> that let us balance spped and cost based on our workload type as follows:
- **Performance-optimized mode (default):** best for <span style="color:rgb(216, 203, 251)">interactive workloads that require fast startup times</span>. Databricks keep a pool of warm compute resources ready.
- **Standard mode:** best for automated batch jobs and <span style="color:rgb(216, 203, 251)">pipelines that can tolerate longer startup times of 4 to 6 minutes</span>.
	- It can reduce costs by up to 70% compared to performance-optimized mode.
# Optimize streaming workloads
Serverless compute supports structure streaming with `Trigger.AvailableNow`. <span style="color:rgb(216, 203, 251)">Time-based trigger intervals are not supported</span>.
## Trigger.AvailableNow optimization
This type of trigger processes all available data in the source which <span style="color:rgb(216, 203, 251)">can result in large micro-batches</span>. 
- To <span style="color:rgb(216, 203, 251)">prevent out-of-memory errors limit the amount of data processed per micro-batch</span> by setting `maxFilesPerTrigger` or `maxBytesPerTrigger`.
# Debug serverless workloads
The Spark UI is not available in serverless compute. Instead, we should use the `query profile`.