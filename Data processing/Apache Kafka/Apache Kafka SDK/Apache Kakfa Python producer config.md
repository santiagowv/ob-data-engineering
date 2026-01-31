# Create the producer properties
```python
from kafka import KafkaProducer
import json

# create the producer properties
producer_config = {
    "bootstrap_servers": "localhost:9092",
    "acks": "all",
    "retries": 4,
    "linger_ms": 10,
    "value_serializer": lambda v: json.dumps(v).encode('utf-8'),
    "key_serializer": lambda k: k.encode('utf-8') if k else None
}
```
# Create the producer
```python
producer = KafkaProducer(**producer_config)
```