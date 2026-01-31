There are aggregation use cases that might require a high degree of control. <span style="color:rgb(216, 203, 251)">State and timers are processed per window</span>.
# State
The State API lets us augment <span style="color:rgb(216, 203, 251)">element-wise operations</span> (for example, `ParDo` or `Map`) with mutable state. 
- When we process each element inside the `ParDo`, we can use the <span style="color:rgb(216, 203, 251)">state variables to write or update state for the current key or to read previous state</span> written for that key.
## Types of state
### ValueState
Scalar state value.
- <span style="color:rgb(216, 203, 251)">For each key in the input, a ValueState stores a typed value</span> that can be read and modified inside the `DoFn`.
### BagState
Allows us to <span style="color:rgb(216, 203, 251)">accumulate elements in an unordered bag</span>. Add elements to a collection <span style="color:rgb(216, 203, 251)">without needing to read any of the previous accumulated elements</span>.
### MapState
Accumulate elements in a <span style="color:rgb(216, 203, 251)">map</span>.
### SetState
Accumulate elements in a <span style="color:rgb(216, 203, 251)">set</span>.
### OrderedListState
Accumulate elements in a <span style="color:rgb(216, 203, 251)">timestamp-sorted list</span>.
# Timers
The Timer API lets us set timers to call back at either an <span style="color:rgb(216, 203, 251)">even-time or a processing-time timestamp</span>.
## Types of timers
### Event-time timers
They fire when the input watermark for the `DoFn` <span style="color:rgb(216, 203, 251)">passes the time at which the timer is set</span>. The runner belives <span style="color:rgb(216, 203, 251)">there are no more elements to be processed</span>.
### Processing-time timers
They fire when the <span style="color:rgb(216, 203, 251)">real wall-clock time passes</span>.
- Often used to <span style="color:rgb(216, 203, 251)">create larger batches of data before processing</span>.
- Used to <span style="color:rgb(216, 203, 251)">schedule events that should occur at a specific time</span>.
### Dynamic timer tags
Set multiple different timers in a `DoFn` and <span style="color:rgb(216, 203, 251)">dynamically choose timer tags</span>.