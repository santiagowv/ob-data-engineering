The <span style="color:rgb(216, 203, 251)">core data structures are immutable</span>, meaning they cannot be changed after they're created.
- Spark will <span style="color:rgb(216, 203, 251)">not act on transformations until we call an action</span>.
```python
divisby2 = myRange.where("number % 2 = 0")
```
# Types of transformations
- Narrow transformations happen through an operation called <span style="color:rgb(216, 203, 251)">pipelining, where they're all peformed in memory</span>.
- With wide transformations Spark will <span style="color:rgb(216, 203, 251)">write the results to disk</span>.
## Narrow dependencies
![[Pasted image 20251225161307.png|350]]
<span style="color:rgb(216, 203, 251)">Each input partition will contribute to only one output</span> partition. Examples of narrow dependencies transformations:
	- `map`
	- `filter`
	- `flatMap`
	- `mapPartitions`
	- `union`
	- `coalesce`
## Wide dependencies
![[Pasted image 20251225162846.png|350]]
It will have <span style="color:rgb(216, 203, 251)">input partitions contributing to many output partitions</span>. This is also <span style="color:rgb(216, 203, 251)">called shuffling whereby Spark will exchange partitions</span> across the cluster.
# Transformations execution plan
We can see the <span style="color:rgb(216, 203, 251)">transformations execution plan</span> calling the `explain`function on the DataFrame object.
```python
flight_data.sort("count").explain()
```
Output:
```
== Physical Plan == 
AdaptiveSparkPlan isFinalPlan=false 
+- Sort [count#40 ASC NULLS FIRST], true, 0 
	+- Exchange rangepartitioning(count#40 ASC NULLS FIRST, 200), ENSURE_REQUIREMENTS, [plan_id=61] 
		+- FileScan csv [DEST_COUNTRY_NAME#38,ORIGIN_COUNTRY_NAME#39,count#40] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[abfss://spark-training@stsparkproject.dfs.core.windows.net/flight-data..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,ORIGIN_COUNTRY_NAME:string,count:int>
```
In this execution plan <span style="color:rgb(216, 203, 251)">sort is a wide transformation</span> because rows will need to be compared with one another.
![[Pasted image 20251225163239.png|550]]
# Lazy evaluation
Spark will <span style="color:rgb(216, 203, 251)">wait until the very last moment to execute</span> the graph of computation instructions.
- Spark compiles the plan from the raw DataFrame transformations to a <span style="color:rgb(216, 203, 251)">streamlined physical plan that will run efficiently</span> across the cluster.
- If we build a large Spark job but specify a filter at the end, Spark will determine that the most efficient way to execute the job is to filter the data first.