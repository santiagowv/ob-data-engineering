![[Pasted image 20260118181927.png|600]]
Data engineers design <span style="color:rgb(216, 203, 251)">systems that choose CP or AP behavior during failures</span>.
# Consitency
Every read <span style="color:rgb(216, 203, 251)">returns the latest commited write</span> (or an error).
# Availability
Every <span style="color:rgb(216, 203, 251)">request gets a reponse</span> (not necessarily the latest).
# Partition tolerance
The system <span style="color:rgb(216, 203, 251)">continues to operate despite network failures between nodes</span>.
# CP
The system preserves correctness. If <span style="color:rgb(216, 203, 251)">it can't guarantee the latest value, it rejects reads/writes</span>.
## Examples
- **PostgreSQL:** If the primary can't confirm replicas, writes fail.
- **HBase:** Strong consistency per row; <span style="color:rgb(216, 203, 251)">regions may be unavailable during partitions</span>.
- **MongoDB:** Read/Writes require a <span style="color:rgb(216, 203, 251)">majority; minority partitions stop serving</span>.
## Use cases
- Financial transactions, billing, inventory.
- Metadata, schemas, coordination services.
# AP
The <span style="color:rgb(216, 203, 251)">system keeps serving request</span>. Data may be temporarily stale (eventual consistency).
## Examples
- **Apache Kafka:** Producers can keep writing even if some broker are unreachable.
- **Apache Cassandra:** Tunable consistency; <span style="color:rgb(216, 203, 251)">can prioritize availability with eventual convergence</span>.
- **Amazon DynamoDB:** Always-on writes; <span style="color:rgb(216, 203, 251)">replicas reconcile asynchronously</span>.
## Use cases
- Event ingestion, logs, metrics.
- Clicksstreams, IoT telemetry.
- HIgh-throughput data lakes ingestion.