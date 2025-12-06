![[Pasted image 20250906062945.png|600]]
# Key considerations when building a data lake
- **Can it handle all the types of data we have?** If we have a RDBMS, we might need to put the data in Cloud SQL, a managed database, rather than cloud storage.
- **Can it scale to meet the demand?** This is more of a problem with on-premises systems than with cloud.
- **Does it support high throughput ingestion?**
	- What is the network bandwidth?
	- Do we have edge points of presence?
- **Is there fine-grained access control to objects?**
- **Can other tools connect easily?**