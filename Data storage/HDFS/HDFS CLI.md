# List files
```powershell
hdfs dfs -ls /
```
# Create folder
```powershell
hdfs dfs -mkdir /input
```
# Copy files to HDFS
```powershell
hdfs dfs -put test.txt /input
```
# See file content
```powershell
hdfs dfs -cat /input/test.txt
```