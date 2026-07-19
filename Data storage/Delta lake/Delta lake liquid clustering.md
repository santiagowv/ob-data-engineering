Physically <span style="color:rgb(216, 203, 251)">groups rows with similar values for selected clustering columns</span> into the same or nearby data files. The <span style="color:rgb(216, 203, 251)">goals is to improve data skipping</span>.
Liquid clustering ensures:
- Uniform file size.
- Appropiate number of files.
- Data always collocated.
# Liquid clustering example
```sql
CLUSTER BY (patient_id)
```
Databricks tries to organize files like:
```
File 1 → patient IDs around 1–1,000
File 2 → patient IDs around 1,001–2,000
File 3 → patient IDs around 2,001–3,000
```
# Liquid clustering query patterns
Layout is not parmanently fixed.
```
CLUSTER BY (patient_id)
```
Later, the query patterns change:
```sql
ALTER TABLE patient_events
CLUSTER BY (provider_id, event_date)
```
`OPTIMIZE` incrementally <span style="color:rgb(216, 203, 251)">organizes data according to the clustering keys</span>. Existing rows are not automatically fully rewritten.
# Liquid clustering vs partioning and z-ordering
<span style="color:rgb(216, 203, 251)">Liquid clustering is more flexible</span> than partioning and z-ordering.
- If the initial filter pattern change we're forced to rewrite the data to adjust to it.
- On the other side, liquid clustering allow for <span style="color:rgb(216, 203, 251)">changes on the clustering columns at any time</span>.

| **Partitioning**                                                               | **Liquid clustering**                                                               |
| ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| Creates <span style="color:rgb(216, 203, 251)">fixed buckets</span>            | <span style="color:rgb(216, 203, 251)">Organizes data within files</span>           |
| Static layout                                                                  | Layout can evolve                                                                   |
| Strong directory-like boundaries                                               | No partition directories required                                                   |
| Good for <span style="color:rgb(216, 203, 251)">low-cardinality</span> columns | Can work with <span style="color:rgb(216, 203, 251)">higher-cardinality</span> keys |
| Risk of over-partitioning                                                      | More <span style="color:rgb(216, 203, 251)">resistant to skew</span>                |
| Changing strategy is painful                                                   | Clustering <span style="color:rgb(216, 203, 251)">keys can be changed</span>        |
| Partition pruning                                                              | Data skipping                                                                       |

## Partitioning limitations
We might partition by one column but if <span style="color:rgb(216, 203, 251)">users commonly query by other columns the data might be spread across hundreds of partitions</span>.
```
event_date=2026-07-03/patient_id=12345/
event_date=2026-07-03/patient_id=12346/
event_date=2026-07-03/patient_id=12347/
```
Partitioning by multiple columns can create huger numbers of tiny partitions.