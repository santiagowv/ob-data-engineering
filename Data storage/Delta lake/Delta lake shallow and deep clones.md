<span style="color:rgb(216, 203, 251)">Create a copy</span> of an existing table at a specific point in time.
- Clones can be created from the latest version of the source table, from a specific table version, or from a specific timestamp.
- Both Shallow clone and Deep clone create a new target table, and <span style="color:rgb(216, 203, 251)">changes made to the clone do not update the source table</span>.
![[Pasted image 20260615163550.png|542]]
# Shallow clone
Creates a new table by copying the <span style="color:rgb(216, 203, 251)">metadata of the source table at a specific point in time</span>, it does no copy the underlying data files. The cloned table references the data files from the source table.
- It's very fast and storage-efficient.
- Useful when we need a <span style="color:rgb(216, 203, 251)">fast, low-cost copy</span> of a table without duplicating data.
## Key characteristics
- <span style="color:rgb(216, 203, 251)">Copies only metadata</span>, not data files.
- Very <span style="color:rgb(216, 203, 251)">fast</span> to create.
- <span style="color:rgb(216, 203, 251)">Uses less storage</span> than a deep clone.
- The <span style="color:rgb(216, 203, 251)">cloned table is independent from the source table</span> from a transaction perspective.
- Updates, deletes, inserts, and merges on the shallow clone <span style="color:rgb(216, 203, 251)">do not update the source table</span>.
- The <span style="color:rgb(216, 203, 251)">clone has its own table history starting from the clone operation</span>.
- The source table's <span style="color:rgb(216, 203, 251)">previous history is not copied to the clone</span>.
- Time travel on the clone <span style="color:rgb(216, 203, 251)">does not use the same versions</span> as the source table.
- A shallow clone depends on the source table's undelrying data file. <span style="color:rgb(216, 203, 251)">If those are removed, the shallow clone may stop working</span>.
## Implementation
<span style="color:rgb(216, 203, 251)">Delta Lake creates a new transaction log for the target table</span>. The initial version of the cloned table represents the selected state of the source table.
- The target table <span style="color:rgb(216, 203, 251)">starts with its own version history</span>.
- The target table <span style="color:rgb(216, 203, 251)">does not inherit the full transaction history</span> of the source table.
- The target table <span style="color:rgb(216, 203, 251)">references the source table's existing data files</span>.
- Future changes to the clone are <span style="color:rgb(216, 203, 251)">tracked in the clone's own delta log</span>.
## Use cases
- Creating a <span style="color:rgb(216, 203, 251)">development or QA copies</span> of production tables.
- Testing transformations without modifying the source table.
- Creating isolated environments with independent permissions.
- Quickly <span style="color:rgb(216, 203, 251)">reproducing the current state</span> of a table.
- Avoiding <span style="color:rgb(216, 203, 251)">unnecessary storage duplication</span>.
## Syntax
- Create a shallow clone from the latest version:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_sc1 SHALLOW CLONE deltacatalog.deltadb.invoices_c1_100;
```
- Create a shallow clone from a specific version:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_sc1 SHALLOW CLONE deltacatalog.deltadb.invoices_c1_100 VERSION AS OF 0;
```
- Create a shallow clone from a specific timestamp:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_sc1 SHALLOW CLONE deltacatalog.deltadb.invoices_c1_100 TIMESTAMP AS OF '2026-06-01';
```
# Deep clone
Creates a full <span style="color:rgb(216, 203, 251)">independent copy of the source table</span> by copying both the metadata and the underlying data files.
- <span style="color:rgb(216, 203, 251)">Does not depend on the original source</span> table after the clone is created.
- Useful when we need a <span style="color:rgb(216, 203, 251)">fully independent physical copy of a table</span>.
## Key characteristics
- Copies <span style="color:rgb(216, 203, 251)">both metadata and data files</span>.
- Uses <span style="color:rgb(216, 203, 251)">more storage than a shallow clone</span>.
- Takes longer to create because <span style="color:rgb(216, 203, 251)">data files must be copied</span>.
- Does <span style="color:rgb(216, 203, 251)">not depend on the source table</span> after creation.
- Changes to the source table <span style="color:rgb(216, 203, 251)">do not affect the deep clone</span>.
- The clone <span style="color:rgb(216, 203, 251)">has its own independent transaction history</span>.
- The source table's <span style="color:rgb(216, 203, 251)">historical delta log is not copied</span>.
## Use cases
- Disaster recovery.
- Long-term archival.
- Creating production-like environments that should not depend on production storage.
- Preserving a dataset version used for ML model training.
- Migrating tables between locations or environments.
- Creating independent copies where source table deletion or vacuum should affect the clone.
## Syntax
- Create a deep clone from the latest version:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_dc1 DEEP CLONE deltacatalog.deltadb.invoices_c1_100;
```
- Since deep clone is the default behavior, this is equivalent:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_dc1 CLONE deltacatalog.deltadb.invoices_c1_100;
```
- Create a deep clone from a specific version:
```sql
CREATE OR REPLACE TABLE deltacatalog.deltadb.invoices_c1_100_dc1 DEEP CLONE deltacatalog.deltadb.invoices_c1_100 VERSION AS OF 0;
```
# Deep clone vs CTAS
A deep clone and `CREATE TABLE AS SELECT` statement can both create a new table with copied data, but <span style="color:rgb(216, 203, 251)">they are not equivalent</span>.
## Deep clone
Copies the table and preserves important Delta table metadata:
- Schema.
- Partitioning.
- Table properties.
- Constraints.
- Nullability.
- Data-specific metdata.
- It's incremental, if the target table already exists, Databricks can <span style="color:rgb(216, 203, 251)">copy only the new metadata and data changes</span> since the previous clone operation.
## CTAS
It <span style="color:rgb(216, 203, 251)">does not automatically preserve all source table metadata</span>. For example, partitioning, table properties, constraints, and other Delta-specific configurations may need to be recreated manually.