---
DBT macro schema name: https://docs.getdbt.com/docs/build/custom-schemas?version=2.0&name=Fusion#how-does-dbt-generate-a-models-schema-name
---
We can configure custom schemas for different groups of modesl.
- <span style="color:rgb(216, 203, 251)">A common pattern is to organize models into subfolders</span> such as `bronze`, `silver`, and `gold`, and then configure each folder to materialize its models in a specific schema.
```yml
models:
  dbt_databricks_project:
    bronze:
      +materialized: table
      +schema: bronze
    silver:
      +materialized: table
      +schema: silver
    gold:
      +materialized: table
      +schema: gold
```