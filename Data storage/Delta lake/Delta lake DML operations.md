<span style="color:rgb(216, 203, 251)">Delta Lake supports DML operations</span>, such as `UPDATE`, `DELETE`, and `MERGE`. This is one of the main advantages of Delta Lake compared to traditional data lakes that only support blob storage.
- Object storage is designed to store files, but <span style="color:rgb(216, 203, 251)">it doesn't understand table-level operations like updating or deleting specific rows</span>.
# Traditional data lake limitation
To perform an update or delete in a traditional data lake, we would typically need to:
1. **Read all the existing data:** this <span style="color:rgb(216, 203, 251)">can be expensive if the dataset is large</span>.
2. **Apply the change in memory:** the <span style="color:rgb(216, 203, 251)">update or delete logic is applied</span> while processing the data.
3. Write the entire dataset back.
<span style="color:rgb(216, 203, 251)">The process may read inconsistent data</span> if another job is writing at the same time.
# How delta lake improves this
It adds a transaction log on top of cloud storage. This <span style="color:rgb(216, 203, 251)">transaction log tracks every change made to the table and allows Delta Lake to manage data</span> as a reliable table of a collection of files.