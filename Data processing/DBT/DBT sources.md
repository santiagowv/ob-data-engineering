---
DBT ref function: https://docs.getdbt.com/reference/dbt-jinja-functions/ref
---
Dependencies are created when a model references either a <span style="color:rgb(216, 203, 251)">source or another upstream model</span>.
# Create a source
Sources are defined in a `.yml` file, usually inside the `models` folder.
```yml
version: 2
sources:
  - name: landing
    database: dbt_project_catalog
    schema: landing
    tables:
      - name: orders
      - name: products
      - name: reviews
      - name: users
```
# Reference a source
To reference a source table, use the `source()` function.
```sql
SELECT
*
FROM
{{ source('landing', 'orders') }}
```
# Reference an upstream model
To reference another dbt model, use the `ref()` function.
```sql
SELECT
*
FROM
{{ ref('bronze_orders') }}
```