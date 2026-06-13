dbt translates SQL models into <span style="color:rgb(216, 203, 251)">SQL queries that are executed in the target data platform</span>, such as:
- Data warehouse.
- Lakehouse.
- Database.
# Modules
Usually a `.sql` file that contains a `SELECT` statement. When the dbt project runs, <span style="color:rgb(216, 203, 251)">dbt compiles the model and executes the resulting SQL against the configured data platform</span>.