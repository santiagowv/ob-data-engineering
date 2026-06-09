Delta Lake supports schema evolution, which <span style="color:rgb(216, 203, 251)">allows the table schema to change over time when new data arrives with additional columns</span> or compatible schema changes.
- This is useful when source systems change their structure, but it <span style="color:rgb(216, 203, 251)">should be used carefully, especially in production environments</span>.
# Adding new columns
Delta Lake can <span style="color:rgb(216, 203, 251)">automatically add new columns to an existing table</span> when schema evolution is enabled.
- <span style="color:rgb(216, 203, 251)">Instead of manually running</span> an `ALTER TABLE` statement to add a new column, we enable automatic schema evolution `spark.conf.set("spark.databricks.delta.schema.autoMerge.enabled", "true")`.
- When `autoMerge` is enabled, Delta Lake can <span style="color:rgb(216, 203, 251)">add new columns from the incoming data to the target table schema</span>.
- For existing records, the new column will be populated with `NULL`.
# Widening data types
<span style="color:rgb(216, 203, 251)">Allows Delta Lake to change a column data type to a wider type</span> that can store all existing values without losing data.
Examples of widening conversion include (these conversions are considered safe):
- `INT` -> `BIGINT`.
- `FLOAT` -> `DOUBLE`.
- `VARCHAR(10)` -> `VARCHAR(20)`.
Type widening should be used carefully because <span style="color:rgb(216, 203, 251)">it changes the table schema and may affect downstream consumers</span>.
# Nested structure evolution
Delta Lake <span style="color:rgb(216, 203, 251)">supports schema evolution for nested structures</span>, such as `STRUCT` columns.
- For example, if a `STRUCT` field receives a new nested attribute, <span style="color:rgb(216, 203, 251)">Delta Lake can evolve the schema to include that new field</span>.
- Existing records will not have a value for the new nested field, so Delta Lake will represent it as `NULL`.
- If automatic schema evolution is enabled, <span style="color:rgb(216, 203, 251)">Delta Lake can handle some nested schema changes</span> without requiring manual `ALTER TABLE` command.
# Column position changes
When adding columns manually, <span style="color:rgb(216, 203, 251)">we can use an</span> `ALTER TABLE` <span style="color:rgb(216, 203, 251)">command to control where the new column appears in the schema</span>.
- Automatic <span style="color:rgb(216, 203, 251)">schema evolution usually adds new columns automatically</span> based on the incoming data, but relying on this behavior in production can be risky.
- If unexpected columns arrive from the source system, they <span style="color:rgb(216, 203, 251)">may be added to the target table without enough control or review</span>.
# Schema evolution in production pipelines
A safer approach is to:
- <span style="color:rgb(216, 203, 251)">Validate incoming schema</span> before writing data.
- <span style="color:rgb(216, 203, 251)">Review schema changes</span> before promoting data to silver or gold layers.
- Use manual `ALTER TABLE` changes for controlled production schema updates.
- Enable automatic evolution only in controlled ingestion layers, such as Bronze, when appropiate.