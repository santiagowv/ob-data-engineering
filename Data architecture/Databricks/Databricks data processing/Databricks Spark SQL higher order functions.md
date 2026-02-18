# Array higher order functions
## Syntax
`<function_name> (array_column, lambda_expression)`.
## Transform
<span style="color:rgb(216, 203, 251)">Transforms elements</span> in an array in `expr` using the function `func`.
```sql
SELECT
order_id,
TRANSFORM(items, x -> UPPER(x)) AS upper_items
FROM order_items;
```
### Nested arrays
```sql
SELECT
order_id,
TRANSFORM(items, x -> named_struct('name' UPPER(x.name),
								   'price', ROUND(x.price * 1.10, 2)
								   )) AS items_with_tax
FROM order_items;								
```
## Filter
<span style="color:rgb(216, 203, 251)">Filters the array</span> in `expr` using the function `func`.
```sql
SELECT
order_id,
FILTER(items, x -> x LIKE '%smart%') AS smart_items
FROM order_items;
```
## Exists
Returns true if `func` is true for any element in `expr` or `query` returns at least one row.
```sql
SELECT
order_id,
EXISTS(items, x -> x = 'monitor') AS smart_items
FROM order_items;
```
## Aggregate
Aggregates elements in an array using a <span style="color:rgb(216, 203, 251)">custom aggregator</span>.
```sql
SELECT
order_id,
AGGREGATE(items, 0, (acc, x) -> acc + x.price) AS total_order_price
FROM order_items;
```
# Map high order functions
## Syntax
`<function_name> (map_column, lambda_expression)`
- Lambda expression: `(key, value) -> expression`
## Transform keys
<span style="color:rgb(216, 203, 251)">Transforms keys</span> in a map in `expr` using the function `func`.
```sql
SELECT
order_id,
transform_keys(item_prices, (k, v) -> UPPER(k)) AS item_prices_fixed
FROM order_item_prices;
```
## Transform values
<span style="color:rgb(216, 203, 251)">Transforms values</span> in a map in `expr` using the function `func`.
```sql
SELECT
order_id,
transform_value(item_prices, (k, v) -> ROUND(v * 1.10, 2)) AS item_prices_with_tax
FROM order_item_prices;
```
## Map filter
<span style="color:rgb(216, 203, 251)">Filters</span> entries in the map in `expr` using the function `func`.
```sql
SELECT
order_id,
map_filter(item_prices, (k, v) -> v > 500) AS items
FROM order_item_prices
```
