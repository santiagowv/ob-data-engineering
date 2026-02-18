- **Insert overwrite:** Use to overwrite the data in a table or a partition when <span style="color:rgb(216, 203, 251)">there are no schema changes</span>.
- **Create or replace table:** Use when <span style="color:rgb(216, 203, 251)">there are schema changes</span>.
# Insert overwrite
More <span style="color:rgb(216, 203, 251)">efficient way to overwrite data</span> without deleting the entire table.
```sql
INSERT OVERWRITE TABLE demo.delta_lake.gold_companies
SELECT * 
	FROM demo.delta_lake.bronze_companies;
```
## Replace specific partition
```sql
INSERT OVERWRITE TABLE demo.delta_lake.gold_companies_partitioned
PARTITION (country = "USA")
SELECT 
	company_name,
	founded_date
FROM demo.delta_lake.bronze_companies_usa;
```