Represent a <span style="color:rgb(216, 203, 251)">data processing operation</span>, or step in the pipeline.
- They're usually applied to one or more input `PCollection` objects.
- The transform logic is <span style="color:rgb(216, 203, 251)">provided in the form of a function object</span>.
- Code is <span style="color:rgb(216, 203, 251)">applied to each element of the input PCollection</span>.
- Many different workers across a cluster might <span style="color:rgb(216, 203, 251)">execute instances of the user code in parallel</span>.
# Transform types
- **Source transforms:** `TextIO.read` and `Create`.
- **Processing and conversion:** `ParDo`, `GroupByKey`, `CoGroupByKey`, `Combine`, and `Count`.
- **Output transforms:** `TextIO.Write`.
- User-defined, application-specific composite transforms.
# Splittable DOFn
The runner has the option of <span style="color:rgb(216, 203, 251)">splitting the element's processing into smaller tasks</span>.
A common computation pattern has the following steps:
1. The runner <span style="color:rgb(216, 203, 251)">splits an incoming element</span> before starting any processing.
2. The runner <span style="color:rgb(216, 203, 251)">starts running the processing logic on each sub-element</span>.
3. If some sub-elements are taking longer than others, <span style="color:rgb(216, 203, 251)">the runner splits those sub-elements further and repeats step 2</span>.
4. The sub-element either <span style="color:rgb(216, 203, 251)">finishes processing</span> or the <span style="color:rgb(216, 203, 251)">user chooses to checkpoint the sub-element</span>.