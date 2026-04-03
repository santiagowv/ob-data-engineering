Allows us to create <span style="color:rgb(216, 203, 251)">dynamic queries</span>.
# Basic syntax
```sql
SELECT * FROM products
WHERE product_id = :product_id AND price > :price;
```
# Parameterize columns
```sql
SELECT
IDENTIFIER(:col1) -- references a column with the parameter value 
FROM products;
```
# Parameterize objects
```sql
SELECT
*
FROM
IDENTIFIER(:catalog || '.' || :schema || '.' || :table);
```