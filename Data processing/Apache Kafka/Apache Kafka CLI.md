# Topics CLI
## Create topic
```powershell
./kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic
```
### Specify partitions
```powershell
./kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic --partitions 5
```
### Replication factor
```powershell
./kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic --replication-factor 2
```
## List topics
```powershell
./kafka-topics.sh --list --bootstrap-server localhost:9092
```
## Describe topic
```powershell
./kafka-topics.sh --bootstrap-server localhost:9092 --topic test-topic --describe
```