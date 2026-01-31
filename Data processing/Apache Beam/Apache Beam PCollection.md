Potentially distirbuted, <span style="color:rgb(216, 203, 251)">homogeneous data set or data stream</span>.
- It's <span style="color:rgb(216, 203, 251)">owned</span> by the specific `Pipeline` object.
- The runner is responsable for <span style="color:rgb(216, 203, 251)">storing these elements</span>.
- Elements of a `Pcollection` usually contain <span style="color:rgb(216, 203, 251)">too much data to fit in memory on a single machine</span> and cannot be processed individually, they are instead processed uniformly in parallel.
# Bounded vs unbounded
Bounded and unbounded PCollections <span style="color:rgb(216, 203, 251)">can coexist in the same pipeline</span>.
## Bounded
Dataset of known, <span style="color:rgb(216, 203, 251)">fixed size</span>.
- It <span style="color:rgb(216, 203, 251)">doesn't grow over time</span>.
- Can be processed by <span style="color:rgb(216, 203, 251)">batch pipelines</span>.
## Unbounded
Dataset that grows over time, and the <span style="color:rgb(216, 203, 251)">elements are processed as they arrive</span>.
- It must be processed by <span style="color:rgb(216, 203, 251)">streaming pipelines</span>.
# Timestamps
Every element in a `PCollection` has a <span style="color:rgb(216, 203, 251)">timestamp associated with it</span>.
- Connectors are responsible for <span style="color:rgb(216, 203, 251)">providing initial timestamps</span>.
- The runner <span style="color:rgb(216, 203, 251)">propagates and aggregates timestamps</span>.
# Watermarks
Every `PCollection` has a watermark that <span style="color:rgb(216, 203, 251)">estimates how complete</span> the `PCollection` is.
- It's the system's notion of <span style="color:rgb(216, 203, 251)">when all data in a certain window can be expected to have arrived in the pipeline</span>.
- If a step fails or stalls, the watermark does not advance.
![[Pasted image 20260103064953.png|600]]
# Windowed elements
Every element in a `PCollection` <span style="color:rgb(216, 203, 251)">resides in a window</span>. Two elements can be equal except for their window.
- When elements are written to the outside world, <span style="color:rgb(216, 203, 251)">they are placed back into the global window</span>.
- A window has a <span style="color:rgb(216, 203, 251)">maximum timestamp</span>. When the watermark exceeds the maximum timestamp plus the allowed lateness, <span style="color:rgb(216, 203, 251)">the window is experied</span>.
- All data related to an expired window might be <span style="color:rgb(216, 203, 251)">discarded at any time</span>.
# Windowing strategy
Every `PCollection` has a windowing strategy, which is a specification of <span style="color:rgb(216, 203, 251)">essential information for grouping and triggering operations</span>.
- The `Window` transform sets up the windowing strategy, and the `GroupByKey` transform has <span style="color:rgb(216, 203, 251)">behavior that is governed by the windowing strategy</span>.