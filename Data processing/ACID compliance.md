It guarantees reliable processing in database management systems.
# A (Atomicity)
All or nothing. Every transaction must be completed entirely or not at all. <span style="color:rgb(216, 203, 251)">If one part fails, the entire transaction fails</span>, and the database remains unchanged.
# C (Consistency)
Data integrity is preserved. <span style="color:rgb(216, 203, 251)">The database moves from one valid state to another</span>, ensuring all written data follows rules like constraints, triggers, and cascades.
- Ensures that all users see the same, correct version of the data.
- Consistency is evaluated based on two criteria:
	- **Timeliness:** whether the data is up-to-date.
	- **Order:** Whether the order of the data updates is preserved.
## Strong consistency
- All queries always return the <span style="color:rgb(216, 203, 251)">most recent data update</span>.
- Requires all parallel processes to <span style="color:rgb(216, 203, 251)">make changes in the same order</span>.
- Trade-off: May result in <span style="color:rgb(216, 203, 251)">slower query responses </span>due to strict synchronization.
- Scenarios requiring <span style="color:rgb(216, 203, 251)">guaranteed accuracy and reliability</span>, such as financial transactions.
## Eventual consistency
- Prioritizes <span style="color:rgb(216, 203, 251)">faster query responses </span>by allowing some updates to be temporarily out of order or delayed.
- Eventually, <span style="color:rgb(216, 203, 251)">all data updates will be visible</span>, achieving a consistent end state over time.
- Trade-off: <span style="color:rgb(216, 203, 251)">Data may be temporarily outdated </span>or unsynchronized in the short term.
- Where speed is prioritized, and immediate accuracy is less critical, such as nationwide census.
# I (Isolation)
Transactions do no interfere with each other.
Even when multiple transactions run simultaneously, <span style="color:rgb(216, 203, 251)">each behaves as if it's the only one</span>, producing the same results as if executed sequentially.
# D (Durability)
Data is permanent.
Once a <span style="color:rgb(216, 203, 251)">transaction has been comitted, it will reamin in the system</span>, even in the event of a system crash or power failure.