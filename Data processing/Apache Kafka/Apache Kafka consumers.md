Consumers <span style="color:rgb(216, 203, 251)">read data from a topic</span> (identified by name).
- Consumers automatically <span style="color:rgb(216, 203, 251)">know which broker to read from</span>.
- In case of broker failures, <span style="color:rgb(216, 203, 251)">consumers know how to recover</span>.
- Data is <span style="color:rgb(216, 203, 251)">read in order from low to high offset</span> within each partitions.
![[Pasted image 20251225183758.png]]
# Consumer deserializer
Deserialize indicates how to <span style="color:rgb(216, 203, 251)">transform bytes into objects/data</span>.
- They are used on the <span style="color:rgb(216, 203, 251)">value and the key of the message</span>.
- Common deserializers.
	- String (incl. JSON).
	- Int, Float.
	- Avro.
	- Protobuf.
- The <span style="color:rgb(216, 203, 251)">serialization/deserialization type must not change during a topic lifecycle</span> (create a new topic instead).
![[Pasted image 20251225184756.png|350]]
# Consumer groups
All the consumers in an application <span style="color:rgb(216, 203, 251)">read data as a consumer group</span>.
- Each consumer within a group <span style="color:rgb(216, 203, 251)">reads from exclusive partitions</span>.
- If we have <span style="color:rgb(216, 203, 251)">more consumers than partitions, some consumers will be inactive</span>.
![[Pasted image 20251225185354.png|600]]
## Multiple consumers on one topic
To create distinct consumer groups, use the consumer property `group.id`.
![[Pasted image 20251225190019.png]]
# Consumer offsets
Kafka stores the <span style="color:rgb(216, 203, 251)">offsets at which a consumer group has been reading</span>.
- The offsets commited are in Kafka topic named __consumer_offsets.
- When a consumer in a group has processed data received from Kafka, it should be <span style="color:rgb(216, 203, 251)">periodically commiting the offsets</span>.
- If a consumer dies, <span style="color:rgb(216, 203, 251)">it will be able to read back from where it left off</span> thanks to the commited consumer offsets.
![[Pasted image 20251225191250.png|700]]
# Delivery semantics for consumers
By default, <span style="color:rgb(216, 203, 251)">Java consumers will automatically commit offsets</span> (at least once).
## Delivery semantics
### At least once (usually preferred)
- Offsets are commited <span style="color:rgb(216, 203, 251)">after the message is processed</span>.
- If the processing goes wrong, <span style="color:rgb(216, 203, 251)">the message will be read again</span>.
- This can result in <span style="color:rgb(216, 203, 251)">duplicate processing of messages</span>. The processing of messages should be idempotent.
### At most once
- Offsets are commited <span style="color:rgb(216, 203, 251)">as soon as messages are received</span>.
- If the processing goes wrong, <span style="color:rgb(216, 203, 251)">some messages will be lost</span> (they won't be read again).
### Exactly once
- **Kafka workflows:** Use the transactional API (easy with Kafka Streams API).
- **External system:** Use an idempotent consumer.