![[Drawing 2026-01-10 08.22.05.excalidraw|700]]
# Databricks subscription model
![[Pasted image 20260110084022.png|650]]
# Compute
It referes to a <span style="color:rgb(216, 203, 251)">cluster of virtual machines</span>.
- The driver node <span style="color:rgb(216, 203, 251)">orchestrates the tasks</span>.
- The worker nodes <span style="color:rgb(216, 203, 251)">execute the processing</span>.
## Serverless
Databricks <span style="color:rgb(216, 203, 251)">releases resources back to the poll</span> when processing is completed.
- Pricing is based on the <span style="color:rgb(216, 203, 251)">cluster's processing time</span>.
## Classic compute
The customer <span style="color:rgb(216, 203, 251)">configures and manages the cluster</span>.
### All purpose
Created <span style="color:rgb(216, 203, 251)">manually</span>.
- Persistent.
- <span style="color:rgb(216, 203, 251)">Interactive</span> workloads.
- <span style="color:rgb(216, 203, 251)">Shared</span> among many users.
- <span style="color:rgb(216, 203, 251)">Expensive</span> to run.
### Job cluster
Created by jobs during <span style="color:rgb(216, 203, 251)">execution time</span>.
- <span style="color:rgb(216, 203, 251)">Terminated</span> at the end of the job.
- <span style="color:rgb(216, 203, 251)">Automated</span> workloads.
- <span style="color:rgb(216, 203, 251)">Isolated</span> just for the job.
- <span style="color:rgb(216, 203, 251)">Cheaper</span> to run.
# Cluster configuration
## Single/Multi node
### Multi node
- <span style="color:rgb(216, 203, 251)">One driver node</span> and <span style="color:rgb(216, 203, 251)">one or more worker nodes</span>.
### Single node
- <span style="color:rgb(216, 203, 251)">One driver node</span> and no worker nodes.
- Ideal for jobs that <span style="color:rgb(216, 203, 251)">don't require distributed processing</span>.
## Access mode
### Dedicated/Single user
<span style="color:rgb(216, 203, 251)">One user access</span>.
- Supports Python, SQL, Scala, R:
### Standard/Shared
<span style="color:rgb(216, 203, 251)">Multiple user access</span>.
- Only <span style="color:rgb(216, 203, 251)">available in Premium</span>, supports Python, SQL.
- Each <span style="color:rgb(216, 203, 251)">process operates in its own environment</span>, preventing shared access between processes.
### No isolation shared
<span style="color:rgb(216, 203, 251)">Multiple user access</span>.
- Supports Python, SQL, Scala, R.
- A <span style="color:rgb(216, 203, 251)">single evironment supports all processes</span>.
## Databricks runtimes
<span style="color:rgb(216, 203, 251)">Core libraries</span> that run on Databricks clusters.
### Databricks runtime
Supports:
- Apache Spark.
- Supporting libraries.
### Databricks runtime ML
Supports:
- Apache Spark.
- Supporting libraries.
- Popular ML libraries.
## Auto termination
Control <span style="color:rgb(216, 203, 251)">unecessary costs on idle clusters</span>.
- <span style="color:rgb(216, 203, 251)">Terminates the cluster</span> after X minutes of inactivity.
- Default value for single node and standard clusters is <span style="color:rgb(216, 203, 251)">120 minutes</span>.
- Users can specify a value between 10 and 43200 mins as the duration.
## Autoscaling
- User specifies the <span style="color:rgb(216, 203, 251)">min and max worker nodes</span>.
- Auto scales between min and max <span style="color:rgb(216, 203, 251)">based on the workload</span>.
- User can opt for <span style="color:rgb(216, 203, 251)">spot instances for the worker nodes</span>.
## Cluster VM Type/Size
### Memory optimized
Suitable for <span style="color:rgb(216, 203, 251)">memory intensive workloads</span>.
### Compute optimized
- Structured streaming applications, where <span style="color:rgb(216, 203, 251)">peak time processing rates are critical</span>.
- <span style="color:rgb(216, 203, 251)">Distributed</span> data workloads.
### Storage optimized
Use cases requiring <span style="color:rgb(216, 203, 251)">high disk throughput and i/o</span>.
### General purpose
<span style="color:rgb(216, 203, 251)">Enterpise grade</span> applications and analytical workloads with in-memory caching.
### GPU accelerated
Deep learning models that are both <span style="color:rgb(216, 203, 251)">data and compute intensive</span>.