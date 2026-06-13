---
DBT Freshness: https://docs.getdbt.com/reference/resource-properties/freshness?version=2.0&name=Fusion
---
It's used to define the acceptable amount of time between the most recevent record, and now, for a <span style="color:rgb(216, 203, 251)">table to be considered fresh</span>.
```yml
version: 2

sources:
	- name: landing
	  database: dbt_project_catalog
	  schema: landing
	  config:
		  # changed to config in v1.9
		freshness:
			warn_after: {count: 12, period: hour}
			error_after: {count: 24, period: hour}
		loaded_at_field: _etl_loaded_at
	
	tables:  
		- name: customers # this will use the freshness defined above  
	  
		- name: orders  
			config:  
				freshness: # make this a little more strict  
					warn_after: {count: 6, period: hour}  
					error_after: {count: 12, period: hour}  
					# Apply a where clause in the freshness query  
					filter: datediff('day', _etl_loaded_at, current_timestamp) < 2  
		- name: product_skus  
			config:  
				freshness: # do not check freshness for this table
```
