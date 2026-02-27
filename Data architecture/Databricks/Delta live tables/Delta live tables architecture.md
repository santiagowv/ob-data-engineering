![[Pasted image 20260226215436.png]]
# Declarative programming
- No need to<span style="color:rgb(216, 203, 251)"> handle checkpoints</span>, <span style="color:rgb(216, 203, 251)">fault tolerance</span>, <span style="color:rgb(216, 203, 251)">dependencies</span>, etc.
- <span style="color:rgb(216, 203, 251)">No need to write complex logic</span> such as CDC, Schema Evolution etc.
- Better <span style="color:rgb(216, 203, 251)">optimization</span>.
![[Pasted image 20260226215953.png]]
# Create live dataset
```sql
CREATE OR REFRESH Live Dataset
	[Data Quality Expectations]
AS
SELECT
	columns,
	transformations
FROM source;
```
# Types of live datasets
## Streaming tables
- Delta table to which <span style="color:rgb(216, 203, 251)">streams write data</span>.
- Reads from <span style="color:rgb(216, 203, 251)">Eventhub, Kafka, Cloud Files, Delta Tables</span>, etc.
- Offers <span style="color:rgb(216, 203, 251)">exactly once guarantees</span>.
### Use cases
- Incremental <span style="color:rgb(216, 203, 251)">data ingestion workloads</span>.
- Low latency <span style="color:rgb(216, 203, 251)">transformations</span>.
- <span style="color:rgb(216, 203, 251)">Massive</span> amount of data.
- <span style="color:rgb(216, 203, 251)">DML</span> operations allowed.
## Materialized views
- Delta table <span style="color:rgb(216, 203, 251)">created from the result of a query</span>.
### Use cases
- <span style="color:rgb(216, 203, 251)">Full refresh</span> data ingestion workloads.
- Build <span style="color:rgb(216, 203, 251)">aggregate tables</span> for reporting.
- <span style="color:rgb(216, 203, 251)">Improve latency</span> of BI reports.
- <span style="color:rgb(216, 203, 251)">Data transformation</span> workloads.
## Views
- No physical storage of data.
- Scope is limited to the pipeline.
- Cannot be published to Unity Catalog.
### Use cases
- Store <span style="color:rgb(216, 203, 251)">intermediate results to reduce complexity</span>.
- Enforce <span style="color:rgb(216, 203, 251)">data quality</span> constraints.