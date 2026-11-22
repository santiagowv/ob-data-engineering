They are Snowflake's implementation of <span style="color:rgb(216, 203, 251)">near real time change data capture (CDC)</span> with the help of which <span style="color:rgb(216, 203, 251)">we can capture data changes like INSERT, UPDATE and DELETE</span> happening in a source.
- Changes can be detected in a <span style="color:rgb(216, 203, 251)">matter of seconds</span> and get promoted to the target table.
![[Pasted image 20261122092942.png]]
# Stream metadata columns
Streams have 2 important metadata columns called `METADATA$ACTION` and `METADATA$ISUPDATE`.
- `METADATA$ACTION`= INSERT
- `METADATA$ACTION` = DELETE.
- `METADATA$ISUPDATE` = TRUE.
- `METADATA$ISUPDATE` = FALSE.