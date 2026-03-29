Allows us to <span style="color:rgb(216, 203, 251)">implement SDC (Slowly changing dimensions)</span> to a streaming table.
# Syntax
It work like a <span style="color:rgb(216, 203, 251)">Merge strategy</span>.
```sql
CREATE OR REFRESH STREAMING TABLE <target_table>;
```
## SQL
```sql
APPLY CHANGES TO <target_table>
	FROM <source_table>
	KEYS <columns>
	SEQUENCE BY <columns>
	STORED AS <SCD TYPE 1 | SCD TYPE 2>
```
### Additional properties
```sql
APPLY CHANGES INTO LIVE.table_name
	FROM source
	KEYS (keys)
	[IGNORE NULL UPDATES]
	[APPLY AS DELETE WHEN condition]
	[APPLY AS TRUNCATE WHEN condition]
	SEQUENCE BY orderByColumn
	[COLUMNS {columnList | * EXCEPT (exceptColumnList)}]
	[STORED AS {SCD TYPE 1 | SCD TYPE 2}]
	[TRACK HISTORY ON {columnList | * EXCEPT (exceptColumnList)}]
```

```sql
CREATE OR REFRESH STREAMING TABLE silver_customers;
APPLY CHANGES INTO LIVE.silver_customers
	FROM STREAM(LIVE.silver_customers_clean)
	KEYS(customer_id)
	SEQUENCE BY created_date
	STORED AS SCD TYPE 1; -- OPtional. Type 1 is the default type.
```
## Python
```python
apply_changes(
	target = "<target-table>",
	source = "<data-source>",
	keys = ["key1", "key2", "keyN"],
	sequence_by = "<sequence-column>",
	ignore_null_updates = False,
	apply_as_deletes = None,
	apply_as_truncates = None,
	column_list = None,
	except_column_list = None,
	stored_as_scd_type = <type>,
	track_history_column_list = None,
	track_history_except_column_list = None
)
```

```python
dlt.create_streaming_table(
	name = "silver_addresses",
	comment = "SCD Type 2 addresses data",
	table_properties = {'quality' : 'silver'}
)

dlt.apply_changes(
	target = "silver_addresses",
	source = "silter_addresses_clean",
	keys = ["customer_id"],
	sequency_by = "created_date",
	stored_as_scd_type = 2
)
```