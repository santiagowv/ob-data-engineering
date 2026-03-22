We can <span style="color:rgb(216, 203, 251)">define tables using fully or partially qualified names</span> in SQL and Python.
# SQL
```sql
-- Same DDL API for creating STREAMING TABLE
CREATE MATERIALIZED VIEW [ `catalog`.`schema`.`name` | catalog.schema.name ] AS ...

-- partially qualified
CREATE MATERIALIZED VIEW [ `schema`.`name` | schema.name ] AS ...

-- single part name
CREATE MATERIALIZED VIEW [ `name` | name ] AS ...
```
# Python
```python
# Similar DDL API for views, sinks, append flows! try it out!
@dlt.table
def table_name():
   ...

# use "name" parameter to create datasets using multipart name
@dlt.table(name="schema.name")
def func():
   ...

@dlt.table(name="catalog.schema.name")
def func():
   ...

# same as DBSQL, if dataset/catalog/schema name has special character, 
# quote the dataset/catalog/schema name
@dlt.table(name="catalog.schema.`name-has-special-character`")
def func():
   ...
```