![[Pasted image 20260315184847.png|630]]
# Syntax
The conditional <span style="color:rgb(216, 203, 251)">expression should evaluate to a boolean value</span>.
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

# Examples
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