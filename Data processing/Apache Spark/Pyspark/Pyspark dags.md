Describes <span style="color:rgb(216, 203, 251)">execution stages and tasks</span> Spark builds from the code so it can run it in parallel across a cluster.
- The driver <span style="color:rgb(216, 203, 251)">builds the DAG</span>.
![[Drawing 2026-02-16 16.31.42.excalidraw|700]]
# How a DAG is built
1. We <span style="color:rgb(216, 203, 251)">write transformations</span>.
2. Spark builds a <span style="color:rgb(216, 203, 251)">logical plan</span>.
3. <span style="color:rgb(216, 203, 251)">Optimizer</span> (Catalyst for SQL/DataFrames) rewrites it.
4. Spark <span style="color:rgb(216, 203, 251)">creates a physical plan</span> (how it will run).
5. When we call an action, <span style="color:rgb(216, 203, 251)">Spark materializes a DAG</span> and splits it into stages.