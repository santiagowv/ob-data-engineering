![[Pasted image 20251224164517.png|650]]
Kafka partitions <span style="color:rgb(216, 203, 251)">distribute a topic across multiple brokers</span>, enabling horizontal scaling, fault tolerance, and higher throughput.
- **Distribution:** Partitioning distributes a topic's data <span style="color:rgb(216, 203, 251)">across multiple brokers</span> in a Kafka cluster.
- **Parallelism:** Partitions enable <span style="color:rgb(216, 203, 251)">parallel processing of messages by consumers</span>, increasing throughput.
- **Scalability:** Partitioning allows topics to <span style="color:rgb(216, 203, 251)">scale beyond the limitations of a single broker</span>.
- **Fault tolerance:** If one broker fails, <span style="color:rgb(216, 203, 251)">partitions on other brokers remain available</span>, ensuring data durability.
- **Ordering:** With a partition, <span style="color:rgb(216, 203, 251)">messages are strictly ordered</span>, providing a clear sequence of events.
# Partition key selection
Choose a partition <span style="color:rgb(216, 203, 251)">key that evenly distributes messages</span> across partitions avoid imbalances.
## Over-partitioning
Excessive partitions can lead to <span style="color:rgb(216, 203, 251)">overhead management complexity</span>.
## Under-partitioning
Too few partitions can <span style="color:rgb(216, 203, 251)">limit throughput and scalability</span>, negating the benefits of partitioning.
## Data locality
Consider data locality to ensure <span style="color:rgb(216, 203, 251)">related data resides within the same partition</span> for efficient processing.
## Consumer group size
The number of <span style="color:rgb(216, 203, 251)">consumers in a consumer group should ideally match the number of partitions for optimal consumption</span>.
# Sticky partitioner
The producer <span style="color:rgb(216, 203, 251)">batches messages and sends them to the same partition</span>.
- It's a <span style="color:rgb(216, 203, 251)">performance improvement feature</span>.
![[Pasted image 20260125165759.png|600]]