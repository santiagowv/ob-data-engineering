```python
future = producer.send(
	topic=topic_name,
	key="user-123",
	value=message
)

future.add_callback(on_send_success)
future.add_callaback(on_send_error)
```