Spark <span style="color:rgb(216, 203, 251)">breaks up the data into chunks</span> called partitions to allow every executor to perform <span style="color:rgb(216, 203, 251)">work in parallel</span>.
- A DataFrame's partitions <span style="color:rgb(216, 203, 251)">represent how the data is physically distributed across the cluster</span> of machines during execution.
- <span style="color:rgb(216, 203, 251)">One partition equals a parallelism of only one</span>, even with thousands of executors.
- Many <span style="color:rgb(216, 203, 251)">partitions but only one executor equals a parallelism of only one</span>.
# Repartition
We can partition our data <span style="color:rgb(216, 203, 251)">according to some frequently filtered columns</span>, which control the <span style="color:rgb(216, 203, 251)">physical layout of data</span> across the cluster including:
- The partitioning scheme.
- The number of partitions.
We should <span style="color:rgb(216, 203, 251)">only repartition when the number of partitions is greater than the current number</span> of partitions.
```python
df.rdd.getNumPartitions()
```
Output:
```
1
```

```python
df.repartition(5)
```
## Frequently filtered columns
It can be worth repartitioning based on a column we will <span style="color:rgb(216, 203, 251)">frequently filter</span>.
```python
df.repartition(col("DEST_COUNTRY_NAME"))
```
Specify the number of partitions:
```python
df.repartition(5, col("DEST_COUNTRY_NAME"))
```
# Coalesce
It tries to <span style="color:rgb(216, 203, 251)">combine partitions</span>.
This operation will shuffle the data into five partitions, and then coalesce them (<span style="color:rgb(216, 203, 251)">without a full shuffle</span>):
```python
df.repartition(5, col("DEST_COUNTRY_NAME")).coalesce(2)
```
# Repartition vs coalesce
- **Shuffle:** `repartition()` 
- **No shuffle:** `coalesce()`
Rule of thumb:
- **Increase partitions:**`repartition()`
- **Decrease partitions:**`coalesce()`
```python
df.coalesce(50)   # cheap, no full shuffle
df.repartition(200)  # expensive, full shuffle
```