![[Pasted image 20260201181729.png]]
Checkpointing is a <span style="color:rgb(216, 203, 251)">fault-tolerance mechanism</span> that allows a query to <span style="color:rgb(216, 203, 251)">recover from failures and resume processing from where it left off</span> without data loss or duplication.
- <span style="color:rgb(216, 203, 251)">Stores metadata</span> about streaming query, execution plan.
- Tracks processed <span style="color:rgb(216, 203, 251)">offsets and commited results</span>.
- <span style="color:rgb(216, 203, 251)">Spark first reads the offset log</span> to get the start and <span style="color:rgb(216, 203, 251)">then the end offset</span> of the previous batch.
- batch id of offset log != batch id of the commit log = the batch failed and <span style="color:rgb(216, 203, 251)">Spark will read the data from the start offset of the previous batch</span>.
