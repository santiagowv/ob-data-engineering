It's a guess as to <span style="color:rgb(216, 203, 251)">when all data in a certain window is expected to have arrived</span> in the pipeline.
- Every `PCollection` must have a <span style="color:rgb(216, 203, 251)">watermark that estimates how complete</span> the `PCollection` is.
- After the watermark progresses past the end of a window, any <span style="color:rgb(216, 203, 251)">element that arrives with a timestamp in that window is late data</span>.
-