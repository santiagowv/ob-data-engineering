![[Pasted image 20250906062945.png|600]]
# Key considerations when building a data lake
- **Can it handle all the types of data we have?** If we have a RDBMS, <span style="color:rgb(216, 203, 251)">we might need to put the data in Cloud SQL, a managed database</span>, rather than cloud storage.
- **Can it scale to meet the demand?** This is more of a <span style="color:rgb(216, 203, 251)">problem with on-premises systems</span> than with cloud.
- **Does it support high throughput ingestion?**
	- What is the <span style="color:rgb(216, 203, 251)">network bandwidth</span>?
	- Do we have <span style="color:rgb(216, 203, 251)">edge points of presence</span>?
- **Is there fine-grained access control to objects?**
- **Can other tools connect easily?**