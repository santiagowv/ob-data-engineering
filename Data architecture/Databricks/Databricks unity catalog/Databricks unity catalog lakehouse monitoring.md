---
Docs Lakehouse monitoring: https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/data-quality-monitoring/
---
# Anomaly detection
Enables <span style="color:rgb(216, 203, 251)">scalable data quality monitoring with one click</span>.
- Analyze historical data patterns to evaluate each table's freshness and completeness.
# Data profiling
Summary statistics of the data in a table. Use it to track the performance of:
- GenAI apps.
- Machine learning models.
- Model-serving endpoints.
![[Pasted image 20260614155549.png|612]]
## How data profiling works
### Time series
Use for tables that contain a time series dataset based on a timestamp column. <span style="color:rgb(216, 203, 251)">Profiling computes data quality metrics across time-based windows of the time series</span>.
### Inference
Tables taht contain the request log for a model. Profiling <span style="color:rgb(216, 203, 251)">compares model performance and data quality metrics across time-based windows</span> of the request log.
### Snapshot
Use for all other types of tables. Profiling <span style="color:rgb(216, 203, 251)">calculates data quality metrics over all data in the table</span>.
- The complete table is processed with every refresh.
- The maximum table size for a snapshot profile is 4 TB.
![[Pasted image 20260614160246.png]]