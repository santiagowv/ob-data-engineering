Allows us to <span style="color:rgb(216, 203, 251)">implement SDC (Slowly changing dimensions)</span> to a streaming table.
# Syntax
It work like a <span style="color:rgb(216, 203, 251)">Merge strategy</span>.
```sql
CREATE OR REFRESH STREAMING TABLE <target_table>;
```

## Basic syntax
```sql
APPLY CHANGES TO <target_table>
	FROM <source_table>
	KEYS <columns>
	SEQUENCE BY <columns>
	STORED AS <SCD TYPE 1 | SCD TYPE 2>
```

## Additional properties
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
# Examples
```sql
CREATE OR REFRESH STREAMING TABLE silver_customers;
```

```sql
APPLY CHANGES INTO LIVE.silver_customers
	FROM STREAM(LIVE.silver_customers_clean)
	KEYS(customer_id)
	SEQUENCE BY created_date
	STORED AS SCD TYPE 1; -- OPtional. Type 1 is the default type.
```