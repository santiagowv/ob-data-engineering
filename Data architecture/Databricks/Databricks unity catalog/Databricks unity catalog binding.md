![[Pasted image 20260518221847.png]]
Organizational and compliance requirements often specify that certain <span style="color:rgb(216, 203, 251)">data must remain accessible only in designated environment</span>.
- <span style="color:rgb(216, 203, 251)">Isolate production data from development</span> or test environments.
- Prevent certain data domains from being joined together.
- Ensure that sensitive data <span style="color:rgb(216, 203, 251)">can only be processed in specific workspaces</span>.
- We can optionally <span style="color:rgb(216, 203, 251)">restrict that workspace to read-only access</span>.
# Best practices
- Use catalogs to segregate information.
- <span style="color:rgb(216, 203, 251)">Owners of production catalogs or schemas should be groups</span>, not users.
- Only gran `USAGE` to objects when we want people to see or query contents.
- Only allow `MODIFY` access to service principals in production.
![[Pasted image 20260518222249.png]]