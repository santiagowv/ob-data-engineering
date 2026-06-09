Delta Lake <span style="color:rgb(216, 203, 251)">uses schema-on-write validation</span>. Delta Lake <span style="color:rgb(216, 203, 251)">validates the structure of the data before writing it</span> to the table.
- The incoming data <span style="color:rgb(216, 203, 251)">must match the target schema, including column names, data types, column order</span> in some cases, and nullability rules.
# Column order validation
When using `INSERT` statements, column order can be important.
- If the `INSERT` statement does not explicitly specify the target columns, <span style="color:rgb(216, 203, 251)">Delta Lake matches values by position</span>.
- <span style="color:rgb(216, 203, 251)">Values can be inserted into the wrong columns</span> if the source column order does not match the target table order.
Example:
```sql
INSERT INTO target_table
SELECT col_a, col_b, col_c
FROM source_table;
```
In this case, Delta Lake inserts the values based on position, not necessarily by column name.
# Data type validation
Delta Lake validates that the <span style="color:rgb(216, 203, 251)">incoming values match the expected data types of the target table</span>.
- If the source data contains a different data type, the <span style="color:rgb(216, 203, 251)">write may fail unless Delta Lake can safely cast or parse the value into the expected type</span>.
- Inserting a numeric string into an integer column may work if the value can be cast correctly.
# Column name validation
With basic `INSERT` statements, <span style="color:rgb(216, 203, 251)">column names are not always used for matching</span>. If target columns are not explicitly specified, Delta Lake matches values by position.
- This means an `INSERT` can still succeed even if the source column names are different, <span style="color:rgb(216, 203, 251)">as long as the number of columns an data types are compatible</span>.
- `MERGE` statements are <span style="color:rgb(216, 203, 251)">safer</span> for this because they reference target columns explicitly. This allows Delta Lake to <span style="color:rgb(216, 203, 251)">validate that the specified target columns actually exist</span>.
# Nullability validation
Delta Lake validates nullability rules when writing data.
- If a column is defined as `NOT NULL`, <span style="color:rgb(216, 203, 251)">Delta Lake should not allow null values to be inserted into that column</span>.
- This <span style="color:rgb(216, 203, 251)">helps protect the table from invalid records</span> that do not satisfy the expected schema constraints.
# Extra columns validation
If an `INSERT` statement includes more columns than the target table expects, <span style="color:rgb(216, 203, 251)">Delta Lake throws an error because the incoming data does not match the target schema</span>.
- For example, if the target table has 5 columns but the source query returns 6 columns, the write will fail.
- With `MERGE` statements, <span style="color:rgb(216, 203, 251)">extra columns in the source data do not necessarily cause an error</span>. Only the columns referenced in the `MERGE` logic are inserted or updated.