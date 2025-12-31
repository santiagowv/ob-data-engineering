![[Pasted image 20251225173854.png]]Producers <span style="color:rgb(216, 203, 251)">write data to topics</span> (which are made of partitions).
- Producers know to <span style="color:rgb(216, 203, 251)">which partition to write to</span> (and which Kafka broker has it).
# Producers message keys
Producers can choose to <span style="color:rgb(216, 203, 251)">send a key with the message</span> (string, number, binary, etc).
![[Pasted image 20251225174211.png|700]]
- If `key=null`, <span style="color:rgb(216, 203, 251)">data is sent round robin</span> (partition 0, then 1, then 2...).
- If `key!=null`, then all <span style="color:rgb(216, 203, 251)">messages for that key will always go to the same partition</span> (hashing).
- Keys are typically <span style="color:rgb(216, 203, 251)">sent if we need message ordering for a specific field</span> (ex. truck_id).
# Kafka message anatomy
![[Pasted image 20251225174704.png|650]]
## Message serializer
Kafka only accepts <span style="color:rgb(216, 203, 251)">bytes as an input from producers</span> and <span style="color:rgb(216, 203, 251)">sends bytes out as an output to consumers</span>.
- Message serialization means <span style="color:rgb(216, 203, 251)">transforming objects/data into bytes</span>.
- They are <span style="color:rgb(216, 203, 251)">used on the value and the key</span>.
- Common serializers.
	- String.
	- Int, Float.
	- Avro.
	- Protobuf.
![[Pasted image 20251225175541.png|400]]