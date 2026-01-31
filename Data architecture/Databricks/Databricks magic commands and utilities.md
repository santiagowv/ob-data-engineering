# Magic commands
Allows us to <span style="color:rgb(216, 203, 251)">change a cell's behavior</span> in a notebook:
- **%python, %sql, %scala, %r:** <span style="color:rgb(216, 203, 251)">Switch to a different language</span> for specific cell.
- **%md:** <span style="color:rgb(216, 203, 251)">Markdown for documenting</span> notebooks.
- **%fs:** <span style="color:rgb(216, 203, 251)">Run file system commands</span>.
- **%sh:** <span style="color:rgb(216, 203, 251)">Run shell commands</span> (Drivery Node only).
- **%pip:** Install Python libraries.
- **%run:** <span style="color:rgb(216, 203, 251)">Include/Import another notebook</span> into the current notebook.
## Run file system commands
Only possible if we've created an <span style="color:rgb(216, 203, 251)">external location</span>.
```
%fs ls 'abfss://gizmobox@deacourseextdl1122026.dfs.core.windows.net/landing/external/payments'
```
List built-in databricks datasets:
```
%fs ls /databricks-datasets/
```
## Run another notebook
```
%run "./2.1 Environment Variables and Functions"
```
```python
print(env)
```
## Install pip libraries
```
%pip install Faker
```
# Utilities
List all available utilities with the `dbutils` object.
```python
dbutils.help()
```
## Use the ls utility
```python
display(dbutils.fs.ls("/"))
```