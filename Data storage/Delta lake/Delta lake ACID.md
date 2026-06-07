Supports ACID transaction, which means it provides reliability and <span style="color:rgb(216, 203, 251)">consistency when reading and writing data</span>.
# Atomicity
Transaction is treated as a single unit of work. The <span style="color:rgb(216, 203, 251)">operation either completes fully or does not happen at all</span>.
- If a job or service fails while writing data, <span style="color:rgb(216, 203, 251)">Delta Lake should not save an incomplete version of the transaction</span>, instead, the <span style="color:rgb(216, 203, 251)">transaction fails and rolls back</span>.
## Example
If a Spark job is writing multiple files to a Delta table and the job fails halfway through, Delta Lake <span style="color:rgb(216, 203, 251)">prevents those partially written files from becoming part of the table</span>.
# Consistency
Every transaction moves the table from one valid state to another valid state.
- Delta Lake <span style="color:rgb(216, 203, 251)">keeps track of the table's state</span> through the transaction log.
- The transaction log records <span style="color:rgb(216, 203, 251)">which files are part of the table</span>, <span style="color:rgb(216, 203, 251)">which files were removed</span>, and <span style="color:rgb(216, 203, 251)">which operations were commited</span>.
- A transaction should only be committed if <span style="color:rgb(216, 203, 251)">it leaves the table in a valid and consistent state</span>.
## Example
If a transaction updates a Delta table, the <span style="color:rgb(216, 203, 251)">table should not be left in a corrupted or incomplete state</span>.
# Isolation
<span style="color:rgb(216, 203, 251)">Multiple transactions can happen at the same time</span> without interfering with each other.
- Readers and writers are isolated from incomplete transactions. <span style="color:rgb(216, 203, 251)">A user or job reading the table won't see intermediate steps from another transaction</span>.
- If two jobs try to modify the same data at the same time, Delta Lake <span style="color:rgb(216, 203, 251)">detects the conflict using optimistic concurrency and prevents inconsistent results</span>.
## Example
If one job is updating a Delta table while another job is reading it, <span style="color:rgb(216, 203, 251)">the reader will continue to see a stable committed version of the table, not the partially updated data</span>.
# Durability
Once a transaction is commited, it is <span style="color:rgb(216, 203, 251)">permanently recorded and should survive system failures</span>.
- Once a transaction is successfully committed, <span style="color:rgb(216, 203, 251)">the table state can be recovered even if there is a cluster failure or service interruption</span>.
- Durability is <span style="color:rgb(216, 203, 251)">also supported by the underlying cloud storage, such as ADLS, S3, or GCS</span>, which provides redundancy and high availability.
## Example
If a transaction is committed and the cluster crashes afterward, <span style="color:rgb(216, 203, 251)">the committed data should still be available when the table is read again</span>.