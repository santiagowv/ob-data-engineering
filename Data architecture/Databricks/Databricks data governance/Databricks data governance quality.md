---
Anomaly detection: https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-quality-monitoring/anomaly-detection/
Data profiling: https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-quality-monitoring/data-profiling/
---
Data quality monitoring in Unity Catalog includes the following capabilities:
# Anomaly detection
Automatically assesses data quality by analyzing data patterns to <span style="color:rgb(216, 203, 251)">evaluate each table's freshness and completeness</span>.
- **Freshness:** analyzes the history of commits to a table and <span style="color:rgb(216, 203, 251)">builds a per-table model to predict the time of the next commit</span>.
- **Completeness:** anomaly detection analyzes the historical row count, and based on this data, <span style="color:rgb(216, 203, 251)">predicts a range of expected number of rows</span>.
## How anomaly works
- It uses default storage to store scan results in the `system.data_quality_monitoring.table_results`.
- Results are <span style="color:rgb(216, 203, 251)">available in Catalog Explorer</span>.
- Databricks <span style="color:rgb(216, 203, 251)">creates a background job that monitors tables for freshness and completeness</span>.
- Intelligent scanning <span style="color:rgb(216, 203, 251)">prioritizes high-impact tables</span> as determined by populary and downstream usage.
## Requirements
- Serverless compute must be available.
- `MANAGE SCHEMA` or `MANAGE CATALOG` priviliges.
## Health indicator status

| Status      | Description                                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| Healthy     | All anomaly detection checks passed in the most recent scan.                     |
| Unhealthy   | One or more checks detected an anomaly, such as freshness or completeness issue. |
| Training    | Anomaly detection is building a baseline model from historical data.             |
| Error       | Anomaly detection encountered and error while monitoring the table.              |
| Excluded    | The table is explicitly excluded from anomaly detection.                         |
| Not enabled | Anomaly detection is not enabled on the schema containing the table.             |
## Limitations
- Anomaly detection <span style="color:rgb(216, 203, 251)">does not support views or foreign tables</span>.
- Determination of completeness does not take into account metrics such as the fraction of nulls, zero values, or NaN.
# Data profiling
![[Pasted image 20260515074017.png]]
Provides <span style="color:rgb(216, 203, 251)">summary statistics of the data in a table</span>.
- Fraction of null or zero values in the current data.
- How does the statistical distribution changes over time.
- Data drift.
## How data profiling works
![[Pasted image 20260515080117.png|662]]

| Profile type | Description                                                                                                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Time series  | Profiling computes <span style="color:rgb(216, 203, 251)">data quality metrics across time-based windows of the time series</span>.                                                      |
| Inference    | <span style="color:rgb(216, 203, 251)">Each row is a request</span>, with columns for the timtestamp, the model inputs, the corresponding prediction, and (optional) ground-truth label. |
| Snapshot     | Profiling <span style="color:rgb(216, 203, 251)">calculates data quality metrics over all data in the table</span>. The maximum table size for a snapshot profile is 4TB.                |
- We can <span style="color:rgb(216, 203, 251)">optionally use a baseline table to use as reference for measuring drift</span>, or change in values over time.
## Requirements
- We must have the following priviliges:
	- `USE CATALOG` on the catalog and `USE SCHEMA` on the schema containing the table.
	- `SELECT` on the table.
	- `MANAGE` on the catalog, schema, or table.
## Limitations
- <span style="color:rgb(216, 203, 251)">Only delta tables are supported for profiling</span>, and the table must be one of the following types: managed tables, external tables, views, materialized views, or streaming tables.
- Profiles created using time series or inference modes <span style="color:rgb(216, 203, 251)">only compute metrics over the last 30 days</span>.
- The <span style="color:rgb(216, 203, 251)">maximum table size for a snapshot profile is 4TB</span>. For larger tables, use time profiles instead.