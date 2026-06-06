Account-level object which represents a <span style="color:rgb(216, 203, 251)">Massively Parallel Processing (MPP) compute cluster</span>.
```sql
CREATE WAREHOUSE my_first_warehouse;
```

```sql
USE WAREHOUSE my_first_warehouse;
```
![[Pasted image 20260525123156.png|649]]
# Virtual warehouse size
- Underlying compute <span style="color:rgb(216, 203, 251)">power approximately doubles with each size</span>.
- In general <span style="color:rgb(216, 203, 251)">the larger the Virtual Warehouse the better the query performance</span>.
- Choosing a size is typically done by <span style="color:rgb(216, 203, 251)">experimenting with a representative query of a workload</span>.
![[Pasted image 20260525123537.png|527]]
## Virtual warehouse state
Warehouse states are:
- Started.
- Suspended.
- Resizing.
### Auto suspend
Specifies the number of <span style="color:rgb(216, 203, 251)">seconds of inactivity after which a warehouse is automatically suspended</span>.
```sql
CREATE WAREHOUSE SMALL_WH
AUTO_SUSPEND=300;
```
### Auto resume
Specifies whether to automatically <span style="color:rgb(216, 203, 251)">resume a warehouse when a SQL statement is submitted to it</span>.
```sql
CREATE WAREHOUSE SMALL_WH
AUTO_RESUME=TRUE;
```
### Initially suspended
Specifies whether the warehouse is <span style="color:rgb(216, 203, 251)">created initially in the Suspended state</span>.
```sql
CREATE WAREHOUSE SMALL_WH
INITIALLY_SUSPENDED=TRUE;
```
