---
DBT Properties: https://docs.getdbt.com/reference/model-properties?version=2.0&name=Fusion
---
# Properties
Models properties can be declared in `.yml` files in the `models/` directory.
## dbt_project.yml
```yml
models:
	dbt_databricks_project:
		+materialized: table
		gold:
			+materialized: view
```
## properties.yml
```yml
version: 2

models:
	- name: gold_sales_daily
	  config:
		  materialized: table
```
## gold_sales__daily.sql model
```sql
{{
    config(
        materialized='view'
    )
}}
SELECT
    o.order_date,
    p.product_name,
    p.category,
    p.vendor,
    u.city,
    u.state,
    u.sales_channel,
    SUM(o.order_amount) AS total_revenue
FROM
{{ ref('silver_orders') }} o
LEFT JOIN {{ ref('silver_products') }} p
ON o.product_id = p.id
LEFT JOIN {{ ref('silver_users') }} u
ON o.user_id = u.id
GROUP BY ALL
```