# send data to a topic
```python
future = producer.send(
    topic = topic_name,
    key = "user-123",
    value = message
)


# optional: wait for acknowledgment
record_metadata = future.get(timeout=10)
print(
    f"Sent to topic={record_metadata.topic}, "
    f"partition={record_metadata.partition}, "
    f"offset={record_metadata.offset}"
)


# flush and close the producer
## tells the producer to send all data and block until done -- synncronous
producer.flush()
producer.close()
```
# Use publisher keys
Keys always <span style="color:rgb(216, 203, 251)">go to the same partition</span>.
```python
future = producer.send(
	topic = topic_name,
	key = "user-123",
	value = message
)
```