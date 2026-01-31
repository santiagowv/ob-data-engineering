Beam uses triggers to determine <span style="color:rgb(216, 203, 251)">when to emit the aggregated results of each window</span> (referred to as a pane).
# Trigger behavior
- By default Beam outputs the aggregated result <span style="color:rgb(216, 203, 251)">when it estimates all data has arrived</span>.
- Triggers allow Beam to emit <span style="color:rgb(216, 203, 251)">early results before all data in a window has arrived</span>.
	- After <span style="color:rgb(216, 203, 251)">certain amount of time elapses</span>.
	- After a <span style="color:rgb(216, 203, 251)">certain number of elements arrives</span>.
- Processing of late data by triggering <span style="color:rgb(216, 203, 251)">after the event time watermark passes the end of the window</span>.
# Types of triggers
## Event time triggers
Operate on the <span style="color:rgb(216, 203, 251)">event time</span>.
## Processing time triggers
Operate on the <span style="color:rgb(216, 203, 251)">processing time</span>, which is the time when the data element is processed at any given stage in the pipeline.
## Data-driven triggers
Examines the data as it arrives in each window, <span style="color:rgb(216, 203, 251)">fires when the data meets a certain property</span>.
## Composite triggers
Combine <span style="color:rgb(216, 203, 251)">multiple triggers in various ways</span>.
- For example, one <span style="color:rgb(216, 203, 251)">trigger for early data and a different trigger for late data</span>.