![[Pasted image 20260315184847.png|630]]
There are three types of expectations:
- Warning.
- Drop.
- Fail
# Syntax
The conditional <span style="color:rgb(216, 203, 251)">expression should evaluate to a boolean value</span>.
## SQL
```
CONSTRAINT <expectation_name>
	EXPECT (<conditional_expression>)
	[ON VIOLATION (FAIL UPDATE | DROP ROW)]
```

```sql
CONSTRAINT valid_customer_id
	EXPECT(customer_id IS NOT NULL)
	ON VIOLATION FAIL UPDATE
```

```sql
CREATE OR REFRESH STREAMING TABLE silver_customers_clean(
  CONSTRAINT valid_customer_id EXPECT (customer_id IS NOT NULL) ON VIOLATION FAIL UPDATE,
  CONSTRAINT valid_customer_name EXPECT (customer_name IS NOT NULL) ON VIOLATION DROP ROW,
  CONSTRAINT valid_telephone EXPECT (LENGTH(telephone) >= 10), -- generates a warning
  CONSTRAINT valid_email EXPECT (email IS NOT NULL), -- generates a warning
  CONSTRAINT valid_date_of_birth EXPECT (date_of_birth >= '1920-01-01') -- generates a warning
)
COMMENT 'Cleaned customers data'
TBLPROPERTIES ('quality' == 'silver')
AS
SELECT
customer_id,
customer_name,
cast(date_of_birth AS DATE) AS date_of_birth,
telephone,
email,
CAST(created_date AS DATE) AS created_date
FROM STREAM(LIVE.bronze_customers) -- treat the table as streaming source and only get the incremental data
```
## Python
- Warning:
```python
@dlt.expect
@dlt.expect_all
```
- Drop:
```python
@dlt.expect_or_drop
@dlt.expect_all_or_drop
```
- Fail:
```python
@dlt.expect_or_fail
@dlt.expect_all_or_fail
```

```python
@dlt.expect("valid_response", "length(postcode) = 5")
@dlt.delete_or_drop("valid_address_line_1", "address_line IS NOT NULL")
@dlt.epect_or_fail("valid_customer_id", "customer_id IS NOT NULL")
```

```python
@dlt.table(
    name = "silver_addresses_clean",
    comment = "Cleaned addresses data",
    table_properties = {'quality' : 'silver'}
)
@dlt.expect_or_fail("valid_customer_id", "customer_id IS NOT NULL")
@dlt.expect_or_drop("valid_address", "address_line_1 IS NOT NULL")
@dlt.expect("valid_postcode", "LENGTH(postcode) = 5")
def create_silver_addresses_clean():
    return (
        spark.readStream.table("LIVE.bronze_addresses") # treat the tables as streaming source and only get the incremental data
            .select(
                "customer_id",
                "address_line_1",
                "city",
                "state",
                "postcode",
                F.col("created_date").cast("date")
            )
    )
```