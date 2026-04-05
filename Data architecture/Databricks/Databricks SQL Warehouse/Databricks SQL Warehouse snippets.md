They are <span style="color:rgb(216, 203, 251)">segments of queries that we can share and trigger using autocomplete</span>. Use query snippets for:
- Frequent <span style="color:rgb(216, 203, 251)">JOIN statements</span>.
- Complicated <span style="color:rgb(216, 203, 251)">clauses like WITH or CASE</span>.
- Conditional <span style="color:rgb(216, 203, 251)">formatting</span>.
Here are examples of snippets:
```sql
--Simple snippet
WHERE fare_amount > 100

--Snippet with an insertion point for a value to be provided at runtime
WHERE fare_amount > ${1:value}

--Snippet with an insertion point for a value to be provided at runtime and containing a default value
WHERE fare_amount > ${1:100}

--Snippet with multiple insertion points
WHERE fare_amount > ${2:min_value} AND fare_amount < ${1:max_value} AND trip_distance < ${0:max_distance}
```