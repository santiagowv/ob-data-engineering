![[Drawing 2026-01-17 13.15.09.excalidraw|700]]
# Catalyst optimizer optimizations
## Filter pushdown
Even if a filter is applied at a later stage in the data pipeline, Spark pushes it down to the data source whenever possible to <span style="color:rgb(216, 203, 251)">avoid processing unnecessary data</span>.
- Even if a predicate is pushed down to the data source, <span style="color:rgb(216, 203, 251)">Spark still includes the predicate in the physical plan</span> to gurantee correctness.
- Spark does not push down filters that involve complex types:
	- Arrays.
	- Maps.
	- Structs.
- Spark does not support pushdown for filters involving a `.cast` operation.
## Pushdown projection
Even when `SELECT *` is used, Spark detects that <span style="color:rgb(216, 203, 251)">only a subset of columns is required for processing and automatically reads only those columns</span>.
# Physical query plans
We get them with the `explain()` method.
```python
df_narrow_transform.explain(True)
```
## Narrow transformations
- `filter` rows where `city='boston'`
- `add` a new column: adding `first_name` and `last_name`
- `alter` an exisitng column: adding 5 to `age` column
- `select` relevant columns
![[Pasted image 20260117133745.png]]
## Wide transformations
### Repartition
```python
df_transactions.rdd.getNumPartitions()

df_transactions.repartition(24).explain(True)
```
![[Pasted image 20260117134435.png]]
`Round-Robin partiotining` distributes <span style="color:rgb(216, 203, 251)">rows cyclically across partitions</span>. Even distribution.
- `isFinalPlan = false`: Indicates that we are running <span style="color:rgb(216, 203, 251)">the explain method on a transformation</span>.
### Coalesce
```python
df_transactions.coalesce(5).explain(true)
```
<span style="color:rgb(216, 203, 251)">The operation only minimizes data movement by merging into fewer partitions</span>, it doesn't do any shuffling.
- <span style="color:rgb(216, 203, 251)">Shuffling occurs when it needs to move data to another executor</span>, for instance, 3 partitions in 3 executors are coalesce into 2 partitions.
![[Pasted image 20260117135330.png]]
### Join
```python
df_joined = (
	df_transactions.join(
		df_customers,
		how = "inner",
		on = "cust_id"
	)
)

df_joined.explain(true)
```
![[Pasted image 20260117140815.png]]
The `Eschange HashPartitioning` is a physical execution step where Spark <span style="color:rgb(216, 203, 251)">reshuffles data across the cluster so that rows with same key end up in the same partition</span>.
- **Formula:** hash(key) % partition_number.
### Group by
```python
df_city_counts = (
	df_transactions
		.groupBy("city")
		.count()	
)
df_city_counts.explain(True)
```
![[Pasted image 20260117142755.png]]
With `HashAggregate` spark uses <span style="color:rgb(216, 203, 251)">partial (on each partition) + final aggregation</span>.
1. `Hash Aggregate`: local no shuffling.
2. `Exchange Hash Partitioning`:  spark <span style="color:rgb(216, 203, 251)">reshuffles data across the cluster so that rows with same key end up in the same partition</span>.
3. `Hash Aggregate`: global result.
 