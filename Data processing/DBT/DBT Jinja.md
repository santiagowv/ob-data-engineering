Templating engine that allows us to <span style="color:rgb(216, 203, 251)">introduce dynamic content into static files like SQL</span>.
- Embed <span style="color:rgb(216, 203, 251)">variables, expressions, loops, and conditional logic</span> directly in the SQL queries.
- Create <span style="color:rgb(216, 203, 251)">reusable and flexible code instead of repeating queries</span> or hardcoding values.
- Re-use code with macros.
- Reference other models dinamically.
```sql
SELECT
*
FROM
{{ ref('model_name') }}
```