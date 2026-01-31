# Querying files
Works for <span style="color:rgb(216, 203, 251)">self-describing schemas</span>.
```sql
SELECT * FROM <file_format>.`<file path>`
```
- **file_format:**
	- CSV
	- JSON
	- XML
	- Parquet
	- Avro
	- ORC
	- Delta
	- Text
	- Binary
- **file path:**
	- Single file.
	- Multiple files with`*`.
	- Folder.
# Query JSON files
## Query single file
```sql
SELECT * FROM json.`/Volumes/gizmobox/landing/operational_data/customers/customers_2024_10.json`
```
## Query multiple files with wildcards
```sql
SELECT * FROM json.`/Volumes/gizmobox/landing/operational_data/customers/customers_2024_*.json`
```
## Query all files within a folder
```sql
SELECT 
_metadata.file_path AS file_path,
* 
FROM json.`/Volumes/gizmobox/landing/operational_data/customers`
```
# Query files as text
Reads the file <span style="color:rgb(216, 203, 251)">in a single text column</span>.
- Ensures <span style="color:rgb(216, 203, 251)">no data loss</span> when working with corrupted files.
```sql
SELECT * FROM text.`/Volumes/gizmobox/landing/operational_data/orders`
```
# Query binary files
Reads the binary data and <span style="color:rgb(216, 203, 251)">creates additional metadata columns</span>.
```sql
CREATE OR REPLACE VIEW gizmobox.bronze.v_customers
AS
SELECT * FROM  binaryFile.`/Volumes/gizmobox/landing/operational_data/memberships/*/*.png`
```
# Query with read_file
We can pass <span style="color:rgb(216, 203, 251)">arguments that change how the data gets read</span>.
```sql
SELECT * FROM read_files('/Volumes/gizmobox/landing/operational_data/addresses',
						 format => 'csv',
						 delimitier => '\t',
						 header => true);
```