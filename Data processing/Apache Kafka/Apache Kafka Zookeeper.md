Zookeeper <span style="color:rgb(216, 203, 251)">manages brokers</span> (kepps a list of them):
- Zookeeper helps in performing <span style="color:rgb(216, 203, 251)">leader election of partitions</span>.
- Zookeeper <span style="color:rgb(216, 203, 251)">sends notifications to Kafka</span> in case of changes (new topic, broker dies, broker comes up).
- Kafka 2.x <span style="color:rgb(216, 203, 251)">can't work without zookeeper</span>.
- Kafka 3.x can work without zookeeper, using <span style="color:rgb(216, 203, 251)">Kafka Raft instead.</span>
- Kakfka 4.x <span style="color:rgb(216, 203, 251)">will not have zookeeper</span>.
- Zookeeper by design operates <span style="color:rgb(216, 203, 251)">with an odd number of servers</span> (1, 3, 5, 7).
- Zookeeper <span style="color:rgb(216, 203, 251)">has a leader</span> (writes) the <span style="color:rgb(216, 203, 251)">rest of the servers are followers</span> (reads).
![[Pasted image 20260111153657.png|650]]
# Kafka Kraft
Zookeeper shows scaling <span style="color:rgb(216, 203, 251)">issues when kafka clusters have > 100.000 partitions</span>.
By removing Zookeeper, Apache Kafka can:
- Scale to millions of partitions, and <span style="color:rgb(216, 203, 251)">becomes easier to maintain and set-up</span>.
- Improve stability, makes it <span style="color:rgb(216, 203, 251)">easier to monitor, support and administer</span>.
- Single <span style="color:rgb(216, 203, 251)">security model for the whole system</span>.
- <span style="color:rgb(216, 203, 251)">Single process to start with kafka</span>.
- Faster controller <span style="color:rgb(216, 203, 251)">shutdown and recovery time</span>.
![[Pasted image 20260111154419.png|650]]