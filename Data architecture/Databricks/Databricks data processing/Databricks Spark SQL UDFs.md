<span style="color:rgb(216, 203, 251)">Apply business logic transformations</span> that are are not possible with built-in functions.
- SQL UDFs are <span style="color:rgb(216, 203, 251)">recommended over Python UDFs</span> due to better optimization.
- UDFs get created in the metastore for <span style="color:rgb(216, 203, 251)">shareable access</span>.
# Syntax
```text
CREATE OR REPLACE FUNCTION catalog_name.schema_name.udf_name(param_name data_type)
RETURNS return_type
RETURN expression;
```
# Example
```sql
CREATE OR REPLACE FUNCTION gizmobox.default.get_fullname(firstname STRING, surname STRING)
RETURNS STRING
RETURN CONCAT(initcap(firstname), " ", initcap(surname))
```