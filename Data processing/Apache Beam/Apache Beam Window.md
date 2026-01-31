Subdivides a `PCollection` into windows according to the <span style="color:rgb(216, 203, 251)">timestamps of its individual elements</span>.
- Windows enable <span style="color:rgb(216, 203, 251)">grouping operations over unbounded collections</span>.
- Each element in a `PCollection` <span style="color:rgb(216, 203, 251)">can only be in one window</span>.
# Types of windowing functions
## Fixed time windows
Also known as <span style="color:rgb(216, 203, 251)">tumbling windows</span>.
- Represent a consistent duration, <span style="color:rgb(216, 203, 251)">non-overlapping time interval in the data stream</span>.
## Sliding time windows
Also known as <span style="color:rgb(216, 203, 251)">hopping windows</span>.
- Represent time intervals in the data stream; however, <span style="color:rgb(216, 203, 251)">sliding time windows can overlap.</span>
## Per-session windows
Define windows that <span style="color:rgb(216, 203, 251)">contain elements that are within a certain gap duration of another element</span>.
## Single global window
All data in a `PCollection` is <span style="color:rgb(216, 203, 251)">assigned to the single global window, and late data is discarded</span>.