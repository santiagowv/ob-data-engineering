# Basic join
```python
flights_with_airports = flights.join(airports, on = "text", how = "inner")

flights_with_airports.show()
```
# Join with conditional operators
## And
```python
df_dedup.join(max_date_by_customer, 
			 (df_dedup.customer_id == max_date_by_customer.customer_id) &
			 (df_dedup.created_timestamp == max_date_by_customer.max_date),
             how = "inner")
```
# Keep columns from one side of the join
```python
df_dedup.join(max_date_by_customer, 
			 (df_dedup.customer_id == max_date_by_customer.customer_id) &
			 (df_dedup.created_timestamp == max_date_by_customer.max_date),
             how = "inner")\
         .select(df_dedup["*"])
```