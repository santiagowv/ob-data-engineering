The `collect` function <span style="color:rgb(216, 203, 251)">executes transformations</span>.
```python
text = sc.textFile("url") # Defines a lazy transformation (no data read yet)
text.collect() # Action that triggers executiong and loads the data
```