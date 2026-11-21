Tasks are <span style="color:rgb(216, 203, 251)">Snowflake's implementation of job scheduling and orchestration</span> where we can run DML, DDL, DROP and invoke stored procedures with the CALL statement based on a schedule.
- Tasks <span style="color:rgb(216, 203, 251)">can run at cyclic intervals</span>. Eg: `SCHEDULE='1 minute'`.
- Tasks <span style="color:rgb(216, 203, 251)">can be scheduled using cron</span>.
- Tasks <span style="color:rgb(216, 203, 251)">can poll a stream to check data</span> and execute SQL statements if there's data in it.
- Creates parent child relationship called <span style="color:rgb(216, 203, 251)">DAG</span>.
- Tasks <span style="color:rgb(216, 203, 251)">cannot create dependencies with external systems</span>.
- Can have a <span style="color:rgb(216, 203, 251)">maximum of 1 SQL statement associated with it</span>.
- Tasks can run for a <span style="color:rgb(216, 203, 251)">maximum of 60 minutes</span> after which it wΩill be aborted.
- It can have a <span style="color:rgb(216, 203, 251)">maximum of 1000 tasks</span> including the root task.
- A task can have a <span style="color:rgb(216, 203, 251)">maximum of 100 child tasks and 100 parent tasks</span>.
![[Pasted image 20261121151624.png|655]]
