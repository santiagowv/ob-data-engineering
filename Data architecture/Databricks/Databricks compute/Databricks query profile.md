We can use a query profile to <span style="color:rgb(216, 203, 251)">visualize the details of a query execution</span>. It helps troubleshoot performance bottlenecks during the query's execution. For example:
- <span style="color:rgb(216, 203, 251)">Time spent</span>, <span style="color:rgb(216, 203, 251)">number of rows processed</span>, and <span style="color:rgb(216, 203, 251)">memory consumption</span>.
- <span style="color:rgb(216, 203, 251)">Slowest part of a query execution</span> at a glance and assess the impacts of modifications to the query.
- Discover and <span style="color:rgb(216, 203, 251)">fix common mistakes in SQL</span> statements.
- We can also <span style="color:rgb(216, 203, 251)">view the query profile in the Spark UI</span>.
![[Pasted image 20261018150856.png|367]]
# Common operations
The most common operations are:
- **Scan:** <span style="color:rgb(216, 203, 251)">data was read from a datasource</span> and output as rows.
- **Join:** <span style="color:rgb(216, 203, 251)">rows from multiple relations were combined</span> (interleaved) into a single set of rows.
- **Union:** <span style="color:rgb(216, 203, 251)">rows from multiple relations that use same schema were concatenated</span> into a single set of rows.
- **Shuffle:** <span style="color:rgb(216, 203, 251)">data was redistributed or repartitioned</span>. Shuffle operations are expensive with regard to resources because they move data between executors on the cluster.
- **Hash/Sort:** <span style="color:rgb(216, 203, 251)">rows were grouped by a key and evaluated using an aggregate function</span> such as `SUM`, `COUNT`, or `MAX` within each group. 
- **Filter:** <span style="color:rgb(216, 203, 251)">input is filtered according to a criteria</span>, such as by a `WHERE` clause.
# Optimize with Genie Code
When a query has insights that we can act on, we can select **Optimize** to open Genie Code.