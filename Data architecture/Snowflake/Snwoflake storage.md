# Columnar storage
Data storage technique that <span style="color:rgb(216, 203, 251)">organizes and stores data blocks</span> by columns instead of rows.
- <span style="color:rgb(216, 203, 251)">Improved query performance</span> and data retrieval.
- Efficient <span style="color:rgb(216, 203, 251)">data compression</span> and <span style="color:rgb(216, 203, 251)">storage saving</span>. Each block holds the same type of data.
- <span style="color:rgb(216, 203, 251)">Less compute needed</span> for processing same amount of data as row storage.
- Use of column metadata to <span style="color:rgb(216, 203, 251)">skip entire sections of data in a table</span> leading to time and compute saving.
- <span style="color:rgb(216, 203, 251)">Efficient parallel loading</span> of data.
# Storage integration
Snowflake object that establishes <span style="color:rgb(216, 203, 251)">2-way connectivity between Cloud Storage (AWS) and Snowflake</span>.
- <span style="color:rgb(216, 203, 251)">Roles are created in AWS</span> to establish trust relationship between snowflake and AWS.
- <span style="color:rgb(216, 203, 251)">Data Files for loading are stored in AWS S3 buckets</span> and are used as a staging or storage point before the COPY statement loads the files to Snwoflake.
![[Pasted image 20261121104038.png|495]]
## Create storage integration
```sql
CREATE OR REPLACE STORAGE INTEGRATION AWS_S3_INT
    TYPE = EXTERNAL_STAGE
    STORAGE_PROVIDER = S3
    ENABLED = TRUE
    STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::1112XXXXX9:role/SNOWFLAKE-S3-FULL-ACCESS'
    STORAGE_ALLOWED_LOCATIONS = ('s3://learn2cloud-snowflake/')
    STORAGE_BLOCKED_LOCATIONS = ('s3://learn2cloud-snowflake/secrete_folder/')
    COMMENT = 'Create Storage integration to connect Snowflake with AWS';
```
# File format
Database object that <span style="color:rgb(216, 203, 251)">stores metadata information about a data file</span>. The information the file format stores includes the file type, delimiter, header information and so on.
## What is the need for a file format
Snowflake uses the `COPY` statement to load files to Snowflake and <span style="color:rgb(216, 203, 251)">creating a file format is how we let Snowflake COPY kno what kind of file to expect when loading data</span>.
## Create a file format
<span style="color:rgb(216, 203, 251)">A named file format can be reused across multiple file loads</span>, tables, stages, and `COPY INTO` commands.
```sql
CREATE OR REPLACE FILE FORMAT HRMS.ETL.CSV_ETL_FILE_FORMAT
TYPE = CSV
FIELD_DELIMITER = ','
SKIP_HEADER = 0
NULL_IF = ('Null', 'NULL')
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
TRIM_SPACE = TRUE
COMPRESSION=AUTO;

DESCRIBE FILE FORMAT HRMS.ETL.CSV_ETL_FILE_FORMAT;
```
# Stage
- Snowflake Stage is a <span style="color:rgb(216, 203, 251)">location on cloud storage where data files are stored</span> (staged) for loading data to Snowflake.
- They are a <span style="color:rgb(216, 203, 251)">place where files can be stored temporarily</span> before they are loaded to Snowflake with the COPY statement.
## External stage
They are created in Snowflake and they <span style="color:rgb(216, 203, 251)">reference storage location on a cloud provider like AWS</span>.
## Internal stage
They are created in Snowflake and they <span style="color:rgb(216, 203, 251)">reference storage locations internally inside Snowflake</span>.
## Data unload
Unloading data means selecting data from Snowflake tables and <span style="color:rgb(216, 203, 251)">creating a file from it on internal or external stages</span>.