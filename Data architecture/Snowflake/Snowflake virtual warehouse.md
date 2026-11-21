Account-level object which represents a <span style="color:rgb(216, 203, 251)">Massively Parallel Processing (MPP) compute cluster</span>.
- <span style="color:rgb(216, 203, 251)">Provides the required resources such</span> as CPU, memory and temporary storage to perform DML and DDL operations.
# Create a virtual warehouse
```sql
CREATE WAREHOUSE my_first_warehouse;
```

```sql
USE WAREHOUSE my_first_warehouse;
```
![[Pasted image 20260525123156.png|649]]
# Virtual warehouse size
- Underlying compute <span style="color:rgb(216, 203, 251)">power approximately doubles with each size</span>.
- In general <span style="color:rgb(216, 203, 251)">the larger the Virtual Warehouse the better the query performance</span>.
- Choosing a size is typically done by <span style="color:rgb(216, 203, 251)">experimenting with a representative query of a workload</span>.
![[Pasted image 20261121095349.png|528]]
## Virtual warehouse state
Warehouse states are:
- Started.
- Suspended.
- Resizing.
### Auto suspend
Specifies the number of <span style="color:rgb(216, 203, 251)">seconds of inactivity after which a warehouse is automatically suspended</span>.
```sql
CREATE WAREHOUSE SMALL_WH
AUTO_SUSPEND=300;
```
### Auto resume
Specifies whether to automatically <span style="color:rgb(216, 203, 251)">resume a warehouse when a SQL statement is submitted to it</span>.
```sql
CREATE WAREHOUSE SMALL_WH
AUTO_RESUME=TRUE;
```
### Initially suspended
Specifies whether the warehouse is <span style="color:rgb(216, 203, 251)">created initially in the Suspended state</span>.
```sql
CREATE WAREHOUSE SMALL_WH
INITIALLY_SUSPENDED=TRUE;
```
# Types of virtual warehouses

| **Warehouse type** | **Main characteristic**                                                                                    | **Best use cases**                              |
| ------------------ | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| Adaptive           | <span style="color:rgb(216, 203, 251)">Automatically allocates and scales compute</span> for each workload | Mixed ETL and analytics, variable workloads     |
| Standard (Gen2)    | <span style="color:rgb(216, 203, 251)">Newer hardware and improved SQL</span> execution                    | SQL analytics, ELT, reporting, data engineering |
| Standard (Gen1)    | <span style="color:rgb(216, 203, 251)">Original generation</span> of Snowflake compute                     | Legacy workloads, existing configurations       |
| Snowpark-optimized | <span style="color:rgb(216, 203, 251)">Increased memory</span> per compute node                            | Python UDFs, Snowpark, ML training              |
| Interactive        | <span style="color:rgb(216, 203, 251)">Low-latency queries</span> and high concurrency                     | Real-time dashboards, applications, APIs        |
