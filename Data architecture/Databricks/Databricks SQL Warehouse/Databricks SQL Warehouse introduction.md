Formerly called Databricks SQL Endpoints. It's a <span style="color:rgb(216, 203, 251)">type of compute</span> within Databricks especialized in SQL Workloads.
- Abstraction layer for <span style="color:rgb(216, 203, 251)">sharing data with other users</span>.
- <span style="color:rgb(216, 203, 251)">Improves query performance</span> for existing data.
# Performance capabilities types
## Classic
 - Basic performance.
 - Manual scaling.
 - Lower cost, fewer optimizations.
## Pro
- Better concurrency handling.
- Improved query execution engine.
- Supports autoscaling.
## Serverless
- Fully managed.
- Fast startup.
- Intelligent scaling.
- Optimized for high <span style="color:rgb(216, 203, 251)">concurrency and low latency</span>.
# Performance features
## Photon
<span style="color:rgb(216, 203, 251)">Makes existing SQL and DataFrame API calls faster</span> and reduces the total cost per workload.
## Predictive IO
Suite of features for <span style="color:rgb(216, 203, 251)">speeding up selective scan operations</span> in SQL queries.
## Intelligent workload management (IWM)
<span style="color:rgb(216, 203, 251)">Process large numbers of queries quickly and cost-effectively</span>. Using AI-powered prediction and dynamic management techniques.
# Sizing
Classic and pro warehouses use a manual scaling model where <span style="color:rgb(216, 203, 251)">we configure the number of clusters</span>.
- We choose a <span style="color:rgb(216, 203, 251)">cluster size</span> and set the <span style="color:rgb(216, 203, 251)">minimum and maximum number of clusters</span>.
- Fixed limit of <span style="color:rgb(216, 203, 251)">one cluster per 10 concurrent queries</span>.
## Queuing and autoscaling logic
Autoscaling <span style="color:rgb(216, 203, 251)">adds clusters based on the estimated time to process all runnig and queued queries</span>.
- 2-6 minutes of query load: add 1 cluster.
- 6-12 minutes: add 2 clusters.
- 12-22 minutes: add 3 clusters.
- Over 22 minutes: add 3 clusters plus 1 more for every additional 15 minutes of load.
- If a <span style="color:rgb(216, 203, 251)">query waits in the queue for 5 minutes, the warehouse scales up</span>.
- If load reamins low for 15 consecutive minutes, <span style="color:rgb(216, 203, 251)">the warehouse scales down to the minimum needed to handle the peak load</span>.