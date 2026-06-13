Used to <span style="color:rgb(216, 203, 251)">manage various aspects of a dbt project</span>:
- Model configurations.
- Sources and tables.
- Testing and documentation.
# Syntax
```yml
name: 'simple_dbt_project'
version: '1.0.0'
source-paths: ["models"]
target-path: "target"
profile: 'my_profile'
models:
	simple_dbt_project:
		+materialized: table
```