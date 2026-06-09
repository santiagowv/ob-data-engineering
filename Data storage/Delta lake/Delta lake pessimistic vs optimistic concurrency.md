# Pessimistic concurrency
Assumes that conflicts between transactions are likely to happen. Because of this, the <span style="color:rgb(216, 203, 251)">system locks the data while a transaction is making changes</span>.
This prevents other users or processes from modifying the same data at the same time:
- Other <span style="color:rgb(216, 203, 251)">transactions must wait until the lock is released</span>.
- It can work well in traditional <span style="color:rgb(216, 203, 251)">databases with frequent row-level updates</span>.
- With a higer number of concurrent transactions, it can slow down the system because <span style="color:rgb(216, 203, 251)">many operations may be waiting for locks</span>.
# Optimistic concurrency
Assumes that conlicts are unlikely to happen. Instead of locking the data before making changes, <span style="color:rgb(216, 203, 251)">multiple transactions can read the data at the same time</span>.
Delta Lake uses this approach.
- Each transaction reads a specific version of the table. Before commiting its changes, Delta Lake <span style="color:rgb(216, 203, 251)">checks whether another transaction has already modified the same data</span>.
- If there is no conflict, <span style="color:rgb(216, 203, 251)">the transaction is committed successfully</span>.
- If there is a conflict, the <span style="color:rgb(216, 203, 251)">transaction fails and must be retried</span>.
- The transaction may need to <span style="color:rgb(216, 203, 251)">re-read the latest table version before applything the changes again</span>.