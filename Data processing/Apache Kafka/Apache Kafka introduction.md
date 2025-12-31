![[Pasted image 20251223200350.png|700]]
Distributed <span style="color:rgb(216, 203, 251)">event-streaming platform</span> that is used for building <span style="color:rgb(216, 203, 251)">real-time</span> data pipelines and streaming applications.
- **High throughput:** Kafka handles massive event streams with ease, ensuring <span style="color:rgb(216, 203, 251)">no message gets left behind</span>.
- **Fault tolerance:** Kafka's distributed architecture guarantees <span style="color:rgb(216, 203, 251)">resilience and high availability</span>, even with failures.
- **Scalability:** Enable instant data and scale the infrastructure to <span style="color:rgb(216, 203, 251)">meet growing demands without service disruptions</span>.
- **Real-time processing:** Instant data insights and actions with Kafka's <span style="color:rgb(216, 203, 251)">low-latency stream processing</span>.
# Non Event-Driven architecture pitfalls
## Tight coupling
Services become <span style="color:rgb(216, 203, 251)">intertwined</span>, hindering independent <span style="color:rgb(216, 203, 251)">updates and flexibility</span>.
## Reduced scalability
Struggle to handle <span style="color:rgb(216, 203, 251)">growing data and user demands</span> due to interdependencies.
## Single point of failure
If one service is down the <span style="color:rgb(216, 203, 251)">entire system may be affected</span>.
## No message persistance
<span style="color:rgb(216, 203, 251)">Lost events mean lost data</span> and potential inconsistencies.
## Limited functionality
Miss out on advanced features for <span style="color:rgb(216, 203, 251)">realiable and efficient event handling</span>.
# Kafka brokers
Brokers handle <span style="color:rgb(216, 203, 251)">storing, retrieving, and distributing messages</span>.
## Cluster node
Each broker is a <span style="color:rgb(216, 203, 251)">machine with a specific capacity in the Kafka cluster</span>. Using multiple brokers ensures high availability and load balancing.
## Scalability
Brokers enable <span style="color:rgb(216, 203, 251)">horizontal scaling</span> by distributing data across nodes.
## Fault tolerance
Redundancy ensures <span style="color:rgb(216, 203, 251)">message durability during broker failures</span>.
## Dynamic membership
Brokers can join or <span style="color:rgb(216, 203, 251)">leave clusters without downtime</span>.