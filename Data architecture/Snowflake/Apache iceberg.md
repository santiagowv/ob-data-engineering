Open-source high performance <span style="color:rgb(216, 203, 251)">open table format for huge analytic tables</span>.
- They're <span style="color:rgb(216, 203, 251)">created in the cloud storage and can be treated just like regular tables</span>, they suport insert, update, delete, truncate, rollback and time travel.
- They can be read by engines like Spark, Presto, Hive and Impala.
- **Hidden partitioning:** <span style="color:rgb(216, 203, 251)">handles partitioning behind the scenes</span>, simplifying data management and optimizing query performance.
- **Snapshot isolation:** ensures <span style="color:rgb(216, 203, 251)">data consistency and integrity</span>, allowing concurrent reads and writes.
	- Only commits changes if the values of the transaction have not changed since the update began.
- **Time travel:** <span style="color:rgb(216, 203, 251)">allows querying of data snpashots at specific point in time</span> (Time Travel), enables rollback and facilitates data audit.
- **File format agnostic:** works with <span style="color:rgb(216, 203, 251)">popular file formats like Parquet, Avro and ORC</span>.
- **Efficient storage management:** <span style="color:rgb(216, 203, 251)">minimizes metadata size and improves read/write peformance</span>, especially for big datasets.