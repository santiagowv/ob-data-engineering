Topics <span style="color:rgb(216, 203, 251)">group</span> related messages logically.
- Like a table in a database (without all the constraints).
- **Immutable**: Messages in a topic are <span style="color:rgb(216, 203, 251)">stored sequentially</span> and can't be <span style="color:rgb(216, 203, 251)">modified</span>, this sequence is also called <span style="color:rgb(216, 203, 251)">data stream</span>.
- **Multi-Consumer access:** Multiple consumers can <span style="color:rgb(216, 203, 251)">read from the same topic</span>.
- **Decoupled Communication:** Producers and consumers <span style="color:rgb(216, 203, 251)">interact through topics</span>, not directly
- **Replication:** Topics ensure data availability with <span style="color:rgb(216, 203, 251)">replication across brokers</span>.
- Data is <span style="color:rgb(216, 203, 251)">kept only for a limited time</span> (default is one week).
# Partitions and offsets
Topics are <span style="color:rgb(216, 203, 251)">split in partitions</span>.
- Messages within each partition are <span style="color:rgb(216, 203, 251)">ordered</span>.
- Each message within a partition gets an incremental id, called <span style="color:rgb(216, 203, 251)">offset</span>.
- Offsets only have a <span style="color:rgb(216, 203, 251)">meaning for a specific partition</span>.
- <span style="color:rgb(216, 203, 251)">Order is guaranteed only within a partition</span> (not across partitions).
- Data is <span style="color:rgb(216, 203, 251)">assigned randomly</span> to a partition unless a key is provided.
- There's no limit for partitions per topic.
![[Pasted image 20251225171145.png|700]]