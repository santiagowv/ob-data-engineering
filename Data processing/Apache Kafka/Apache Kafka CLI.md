# Topic CLI
## Create topic
```powershell
kafka-topics --bootstrap-server localhost:9092 --create --topic test-topic
```
### Specify partitions
```powershell
kafka-topics --bootstrap-server localhost:9092 --create --topic test-topic --partitions 5
```
### Replication factor
```powershell
kafka-topics --bootstrap-server localhost:9092 --create --topic test-topic --replication-factor 2
```
## List topics
```powershell
kafka-topics --bootstrap-server localhost:9092 --list
```
## Describe topic
```powershell
kafka-topics --bootstrap-server localhost:9092 --topic test-topic --describe
```
# Producer CLI
## Producing to a topic
```powershell
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
> Hello world
> My name is Santiago
```
## Producing with properties
```powershell
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic --producer-property acks=all
> Hello world
> Just for fun
```
## Producing with keys
```powershell
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic --property parse.key=true --property key.separator=:
>example key:example value
>name:Stephane
```
## Producing with RounRobin
<span style="color:rgb(216, 203, 251)">Distributes messages evenly</span> across all partitions of the topic, ignoring the message key.
```powershell
kafka-console-producer --bootstrap-server localhost:9092 --producer-property partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner --topic second_topic
```
# Consumer CLI
## Consume from topic
```powershell
kafka-console-consumer --bootstrap-server localhost:9092 --topic second_topic
```
## Consume from beginning
Ordering is only guaranteed when using a <span style="color:rgb(216, 203, 251)">single partition topic</span>.
```powershell
kafka-console-consumer --bootstrap-server localhost:9092 --topic second_topic --from-beginning
```
## Consume with metadata
Display key, values and timestamp in consumer.
```powershell
kafka-console-consumer --bootstrap-server localhost:9092 --topic second_topic --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --property print.partition=true --from-beginning
```
# Consumer groups
## Start one consumer group
```powershell
kafka-console-consumer --bootstrap-server localhost:9092 --topic third_topic --group my-first-application
```
## Spread messages between consumers
Consumers of the same group <span style="color:rgb(216, 203, 251)">will get assigned to a specific topic partition</span>.
```powershell
kafka-console-consumer --bootstrap-server localhost:9092 --topic third_topic --group my-first-application
```
## List all consumer groups
```powershell
kafka-consumer-groups --bootstrap-server localhost:9092 --list
```
## Describe consumer group
```powershell
kafka-consumer-groups --bootstrap-server localhost:9092 --describe -group my-first-application
```
## Reset consumer group offset
Reset the consumer group offset so that <span style="color:rgb(216, 203, 251)">all messages have to be read again</span>.
```powershell
kakfa-consumer-groups --bootstrap-server localhost:9092 --reset-offsets --to-earliest --topic third_topic --dry-run
```
```powershell
kakfa-consumer-groups --bootstrap-server localhost:9092 --reset-offsets --to-earliest --topic third_topic --execute
```
