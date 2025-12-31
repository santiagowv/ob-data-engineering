# Cluster
It's a <span style="color:rgb(216, 203, 251)">machine with a specific processing capacity</span>.
![[Pasted image 20251225095643.png|400]]
# Spark applications
![[Pasted image 20251225071513.png|500]]
Consist of a <span style="color:rgb(216, 203, 251)">driver process</span> and a set of <span style="color:rgb(216, 203, 251)">executor processes</span>.
## Driver process
It runs the `main()` function, <span style="color:rgb(216, 203, 251)">sits on a node</span> in the cluster.
- <span style="color:rgb(216, 203, 251)">Maintains information</span> about Spark applications.
- <span style="color:rgb(216, 203, 251)">Responds</span> to a user's program or input.
- <span style="color:rgb(216, 203, 251)">Analyzes, distributes and schedules</span> work across the executors.
## Executors
They are responsible for <span style="color:rgb(216, 203, 251)">carrying out the work that the driver assigns them</span>.
- <span style="color:rgb(216, 203, 251)">Executes the code</span>assigned to it by the driver.
- Reports the <span style="color:rgb(216, 203, 251)">state of the computation</span> on that execution back to the driver.
## Cluster manager
<span style="color:rgb(216, 203, 251)">Controls physical machines</span> and <span style="color:rgb(216, 203, 251)">allocates resources</span> to Spark Applications. They can be:
- Sparks' <span style="color:rgb(216, 203, 251)">standalone cluster manager</span>.
- YARN.
- Mesos.
# Execution
1. <span style="color:rgb(216, 203, 251)">Write</span> DataFrame/Dataset/SQL code
2. If valid code, Spark <span style="color:rgb(216, 203, 251)">converts this to a logical plan</span>.
3. Spark <span style="color:rgb(216, 203, 251)">transforms the logical plan to a physical plan</span>, checking for optimization along the way.
4. Spark then executes this Physical Plan (RDD manipulations) on the cluster.
## Logical planning
Represents a set of <span style="color:rgb(216, 203, 251)">abstract transformations that do not refer to executors or drivers</span>. It converts the user's set of expressions into the most optimized version.
- Converts user code into an <span style="color:rgb(216, 203, 251)">unresolved logical plan</span>.
- Spark uses the catalog, a <span style="color:rgb(216, 203, 251)">repository of all table and DataFrame information</span>, to resolve columns and tables in the analyzer.
![[Pasted image 20251225073232.png|650]]
## Physical planning
Specifies <span style="color:rgb(216, 203, 251)">how the logical plan will execute on the cluster</span> by generating different physical execution strategies and comparting them through a cost model.
![[Pasted image 20251225073530.png|600]]