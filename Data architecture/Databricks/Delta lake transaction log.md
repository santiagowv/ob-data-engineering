Every transaction <span style="color:rgb(216, 203, 251)">records the operation</span> in the delta transaction log.
1. Transaction logs are <span style="color:rgb(216, 203, 251)">written at the end of the transaction</span>.
2. <span style="color:rgb(216, 203, 251)">Readers will always read the transaction logs first</span> to identify the list of data files to read.
![[Pasted image 20260208172556.png|700]]
# ACID Transactions
<span style="color:rgb(216, 203, 251)">Partially written data doesn't create a transaction log</span>, readers get the latest version of the data from the transaction log only.
![[Pasted image 20260208183521.png|650]]