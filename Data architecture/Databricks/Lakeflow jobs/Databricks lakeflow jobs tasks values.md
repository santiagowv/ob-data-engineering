<span style="color:rgb(216, 203, 251)">Pass arbitrary values between tasks</span> in a Databricks job.
# Set a task value
```python
dbutils.jobs.taskValues.set("total_records", total_records)
```
# Get a task value
```python
dbutils.widgets.get("total_records")
```
# Get job task values
```python
dbutils.jobs.taskValues.get("task-x", "total_records")
```
# Pass SQL outputs as parameters
```json
{{ tasks.SQLQuery.output.rows }}
```
# Reference task values in task definition
```
{{tasks.`task-x`.values.total_records}}
```
## Reference task values from loop tasks
```
{{input.file_name}}
```