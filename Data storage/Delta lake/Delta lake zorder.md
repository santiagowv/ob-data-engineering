Rewrites files and <span style="color:rgb(216, 203, 251)">co-locates similar values together</span> across one or more columns using Z-ordering, which can <span style="color:rgb(216, 203, 251)">improve data skipping for filters on those columns</span>.
![[Drawing 2026-07-12 16.07.50.excalidraw]]
# Z-order Example
Z-order does not create a folder for every `member_id`. Instead, it <span style="color:rgb(216, 203, 251)">places similar</span> `member_id` <span style="color:rgb(216, 203, 251)">values close together</span> in the same files.
```sql
OPTIMIZE claims
ZORDER BY (member_id);
```
# Z-order by multiple columns
Databricks tries to co-locate <span style="color:rgb(216, 203, 251)">rows with similar combinations of values accross all specified columns</span>.
```sql
OPTIMIZE claims
ZORDER BY (member_id, service_date);
```
Conceptually, Databricks:
1. Takes values from `member_id` and `service_date`.
2. Maps the multidimensional values onto a one-dimensional Z-order curve.
3. <span style="color:rgb(216, 203, 251)">Writes nearby combinations</span> into the same files.
4. Uses each file's <span style="color:rgb(216, 203, 251)">min/max statistics to skip files</span> during queries.
# Z-order vs partitioning

| Aspect          | Partitioning                                                                             | Z-Order                                                                              |
| --------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Physical layout | Creates <span style="color:rgb(216, 203, 251)">separate directory-like partitions</span> | <span style="color:rgb(216, 203, 251)">Reorganizes rows</span> inside data files     |
| Best columns    | <span style="color:rgb(216, 203, 251)">Low-cardinality</span> columns                    | <span style="color:rgb(216, 203, 251)">Medium/high-cardinality</span> filter columns |
| Examples        | `load_date`, `year`, `region`                                                            | `patient_id`, `member_id`, `claim_id`                                                |
| Query benefir   | Skips entire partitions                                                                  | Skips files using min/max statistics                                                 |
| Applied when    | Table is created or written                                                              | Through `OPTIMIZE ... ZORDER BY`                                                     |
| Main risk       | Too many small partitions/files                                                          | <span style="color:rgb(216, 203, 251)">Requires periodic optimizacion</span>         |
## Partitioning works best when
Use partitioning for broad, predictable filters.
- Queries almost always <span style="color:rgb(216, 203, 251)">filter by the partition column</span>.
- Each partition contains a <span style="color:rgb(216, 203, 251)">substantial amount of data</span>.
- The column has <span style="color:rgb(216, 203, 251)">relatively few distinct values</span>.
## Z-ordering works best when
- The filer columns have <span style="color:rgb(216, 203, 251)">high cardinality</span>.
- Queries <span style="color:rgb(216, 203, 251)">filter on multiple different columns</span>.
- Query filers <span style="color:rgb(216, 203, 251)">include ranges</span> or selective identifiers.