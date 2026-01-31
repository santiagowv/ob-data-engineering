AQE (Adaptive Query Execution) and broadcast joins were <span style="color:rgb(216, 203, 251)">included in SPark 3.0</span>.
# AQE Adaptive Query execution
Spark <span style="color:rgb(216, 203, 251)">uses runtime statistics to select the best execution plan</span>.
![[Drawing 2026-01-18 15.40.21.excalidraw|600]]
# Broadcast Joins
Useful when <span style="color:rgb(216, 203, 251)">merging one big table with a significantly smaller table</span>.
1. Spark build the <span style="color:rgb(216, 203, 251)">logical plan</span>.
2. Spark checks sizes + picks a join strategy.
3. Spark <span style="color:rgb(216, 203, 251)">creates a physical plan</span>.
4. Spark <span style="color:rgb(216, 203, 251)">collects the small table on the driver</span>.
5. Spark <span style="color:rgb(216, 203, 251)">broadcasts the small table to all executors</span>.
6. Each executor <span style="color:rgb(216, 203, 251)">builds a local hash table</span>.
7. Spark <span style="color:rgb(216, 203, 251)">scans the big table in parallel</span>.
8. Join happens <span style="color:rgb(216, 203, 251)">locally per partition</span>.
9. <span style="color:rgb(216, 203, 251)">Result partitions</span> are produced.
10. Only <span style="color:rgb(216, 203, 251)">later shuffles happen if the next operation requires them</span>.