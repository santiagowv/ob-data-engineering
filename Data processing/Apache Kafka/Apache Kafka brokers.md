A kafka cluster is <span style="color:rgb(216, 203, 251)">composed of multiple brokers</span> (servers).
- Each broker is <span style="color:rgb(216, 203, 251)">identified with its ID</span> (integer).
- Each broker <span style="color:rgb(216, 203, 251)">contains certain topic partitions</span>.
- After connecting to a any broker (called a bootstrap broker), we will be <span style="color:rgb(216, 203, 251)">connected to the entire cluster</span> (Kafka clients have smart mechanics for that).
- A good number to <span style="color:rgb(216, 203, 251)">get started is 3 brokers</span>.
![[Pasted image 20260111111747.png|700]]
# Kafka broker discovery
- Every kakfa broker is also called a **bootstrap server**.
- Each broker <span style="color:rgb(216, 203, 251)">knows about all brokers</span>, topics and partitions (metadata).
![[Pasted image 20260111112046.png|700]]
# Topic replication factor
Topics should have a <span style="color:rgb(216, 203, 251)">replication factor > 1</span> (usually between 2 and 3).
- If a broker is down, <span style="color:rgb(216, 203, 251)">another broker can serve the data</span>.
- Example: Topic-A with <span style="color:rgb(216, 203, 251)">2 partitions and replication factor of 2</span>.
![[Pasted image 20260111113032.png|650]]
We lose broker 102:
![[Pasted image 20260111113140.png|650]]
# Leader for a partition
At any time <span style="color:rgb(216, 203, 251)">only one broker can be a leader for a given partition</span>.
- Producers can only <span style="color:rgb(216, 203, 251)">send data to the broker that is the leader of a partition</span>.
- The other brokers will <span style="color:rgb(216, 203, 251)">replicate the data</span>.
- Therefore, each partition has <span style="color:rgb(216, 203, 251)">one leader and multiple ISR</span> (in-sync replica).
![[Pasted image 20260111113448.png|650]]
## Producers and consumers with leaders
- Kakfa producers can only <span style="color:rgb(216, 203, 251)">write to the leader for a partition</span>.
- Kakfa consumers by default will <span style="color:rgb(216, 203, 251)">read from the leader broker for a partition</span>.
![[Pasted image 20260111113837.png|600]]
### Replica fetching
Since <span style="color:rgb(216, 203, 251)">Kafka 2.4</span>, it is possible to configure <span style="color:rgb(216, 203, 251)">consumers to read from the closest replica</span>.
- This may help <span style="color:rgb(216, 203, 251)">improve latency</span>, and <span style="color:rgb(216, 203, 251)">decrease network cost</span>.
![[Pasted image 20260111114000.png|700]]
# Producer acknowledgements (acks)
Producers can choose to receive acknowledgment of data writes:
- **acks=0:** producer <span style="color:rgb(216, 203, 251)">won't wait for acknowledgment</span> (possible data loss).
- **acks=1:** producer will <span style="color:rgb(216, 203, 251)">wait for leader acknowledgment</span> (limited data loss).
- **acks=all:** <span style="color:rgb(216, 203, 251)">leader+replicas acknowledgement</span> (no data loss).
## Topic durability
For a topic replication of 3, topic data durability can <span style="color:rgb(216, 203, 251)">withstand 2 brokers loss</span>.
- For a replication factor of N, we can <span style="color:rgb(216, 203, 251)">permanently lose up to N-1 brokers and still recover</span> the data.
![[Pasted image 20260111114754.png|650]]
