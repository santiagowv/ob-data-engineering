They're used to apply <span style="color:rgb(216, 203, 251)">custom logic</span> to a `PTransform`.
- When using `ParDo`, it specifies <span style="color:rgb(216, 203, 251)">what operation to apply to every element</span>.
- When using `Combine`, it specifies <span style="color:rgb(216, 203, 251)">how values should be combined</span>.
# Types of UDFs
## DoFn
<span style="color:rgb(216, 203, 251)">Per element processing</span> function (used in `ParDo`).
## WindowFn
Places elements in windows and <span style="color:rgb(216, 203, 251)">merges windows</span> (used in `Window` an `GroupByKey`).
## ViewFn
Adapts a materialized `PCollection` to a <span style="color:rgb(216, 203, 251)">particular interface</span> (used in side inputs).
## WindowMappingFn
Maps one element's window to another, and specifies <span style="color:rgb(216, 203, 251)">bounds how far in the past the result window will be</span> (used in side inputs).
## CombineFn
<span style="color:rgb(216, 203, 251)">Associative</span> and <span style="color:rgb(216, 203, 251)">commutative</span> aggregation (used in `Combine` and state).