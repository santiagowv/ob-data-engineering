# DBT compile
<span style="color:rgb(216, 203, 251)">Compile all SQL</span> files in the project.
# DBT run
Compile all SQL files and <span style="color:rgb(216, 203, 251)">run the models in the project</span>.
## Run specific models
```
dbt run --select "bronze_orders"
```
## Run specific dependencies
Runs model and the next downstream dependency.
```sql
dbt run --select "bronze_orders+1"
```
## Exclude specific models
```sql
dbt run --exclude "gold_sales_daily"
```
# DBT clean
<span style="color:rgb(216, 203, 251)">Delete target folders</span> where compile code and run code gets created.