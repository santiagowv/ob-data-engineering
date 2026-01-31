In Beam, aggregation happens by <span style="color:rgb(216, 203, 251)">grouping all elements with a common key and window</span>, then combining each group of elements. Governing <span style="color:rgb(216, 203, 251)">when to emit the results of aggregations</span> depends on:
- **Windowing:** Partitions input into <span style="color:rgb(216, 203, 251)">bounded subsets</span>.
- **Watermarks:** Estimate <span style="color:rgb(216, 203, 251)">completeness of the input</span>.
- **Triggers:** When and <span style="color:rgb(216, 203, 251)">how to emit aggregated results</span>.
# GroupByKey
When <span style="color:rgb(216, 203, 251)">elements are grouped and emitted as a bag</span>.
- The output is no smaller than the input.
# CombineFn
The output is <span style="color:rgb(216, 203, 251)">smaller than the input</span>.
# Parallelism
In cases when we have fewer keys, we can <span style="color:rgb(216, 203, 251)">add a supplementary key</span>, splitting each natural key into many sub-keys. After these sub-keys are aggregated, the <span style="color:rgb(216, 203, 251)">results are combined into the original natural key</span>.