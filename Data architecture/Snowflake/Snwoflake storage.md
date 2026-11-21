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
```sql
CREATE OR REPLACE STAGE HRMS.ETL.AWS_ETL_CSV_STAGE
STORAGE_INTEGRATION = AWS_S3_INT
URL = 's3://learn2cloud-snowflake20261121/loadingdata/csv/'
FILE_FORMAT = HRMS.ETL.CSV_ETL_FILE_FORMAT;

DESCRIBE STAGE HRMS.ETL.AWS_ETL_CSV_STAGE;
```
## External stage
They are created in Snowflake and they <span style="color:rgb(216, 203, 251)">reference storage location on a cloud provider like AWS</span>.
## Internal stage
They are created in Snowflake and they <span style="color:rgb(216, 203, 251)">reference storage locations internally inside Snowflake</span>.
## Data unload
Unloading data means selecting data from Snowflake tables and <span style="color:rgb(216, 203, 251)">creating a file from it on internal or external stages</span>.
# Load types
## Bulk loading
It's the process of loading source system data file in batches from cloud storage to tables in Snowflake at <span style="color:rgb(216, 203, 251)">regular intervals of time</span>.
- Bulk loading uses COPY statement.
- Set up a scheduler to run the COPY in an interval.
- Data latency is higher.
- It's a simpler and lower cost option.
- Supports JSON, AVRO, ORC, PARQUET, XML and CSV.
## Near real time loading
Refers to process of frequently <span style="color:rgb(216, 203, 251)">ingesting data into tables with low latency</span>.
- It's <span style="color:rgb(216, 203, 251)">event driven</span> and runs on the occurrence of the event which is the arrival of a file on cloud storage.
- Near real time loading is <span style="color:rgb(216, 203, 251)">done with Snowpipe</span> in Snowflake.
- Files processed in near real time loads tend to be <span style="color:rgb(216, 203, 251)">smaller than those used in bulk loads</span>.
- <span style="color:rgb(216, 203, 251)">Credit savings</span> in cases when files appear intermittently.
## Real time loading
Involves <span style="color:rgb(216, 203, 251)">continuously ingesting data into snowflake tables</span> immediately as it is created or updated in the source systems.
- Latency is a few seconds.
- There is <span style="color:rgb(216, 203, 251)">no scheduling</span>.
- It's done with the help of <span style="color:rgb(216, 203, 251)">Apache Kafka</span> na dusing a Snowflake connector for Apache Kafka.
- <span style="color:rgb(216, 203, 251)">Higher cost</span> when compared to builk loading and near real time loading.
- Only supports Avro and JSON file formats.
# COPY statement
It's used to <span style="color:rgb(216, 203, 251)">load data from Snowflake stages</span> (External or Internal) to tables in Snowflake. It can also be used to unload data from Snowflake tables to external stages or internal stages
- Loads all the files inside the stage to the target table.
- Any file that is loaded to the target table will not be re-loaded if the `COPY` statement is re-run.
![[Pasted image 20261121141804.png|524]]

```sql
COPY INTO EMPLOYEES
FROM @HRMS.HR.AWS_LOAD_CSV_STAGE
ON_ERROR=CONTINUE;
```
## Copy options
It <span style="color:rgb(216, 203, 251)">alters the default behavior</span> of the `COPY` statement. The default behavior is that when COPY encounteers an error it aborts the run.
# Semi-strucuted data
## Object
<span style="color:rgb(216, 203, 251)">Consits of one or more key value pairs</span> where each key is a unique string and value can be provided for this unique string or key.
```json
{
	"name":"john",
	"age":30,
	"skill":"SQL"
}
```

```sql
SELECT
	e:name,
	e:age,
	e:skill
FROM EMPLOYEE_DATA e;
```
## Array
Ordered collection of elements <span style="color:rgb(216, 203, 251)">stored in a list of values</span>. The elements in the array have a specific sequence and we can access each element by its position (index).
```sql
SELECT
	skills[0],
	skills[1],
	skills[2]
FROM EMPLOYEE_DATA;
```
## Variant
Superset of OBJECT and ARRAY. It can <span style="color:rgb(216, 203, 251)">store both OBJECT and ARRAY data</span> either individually or combined together.
```json
{
  "skills": [
    {
      "skill_name": "Project Management",
      "proficiency_level": "Expert"
    },
    {
      "skill_name": "Agile Methodologies",
      "proficiency_level": "Advanced"
    },
    {
      "skill_name": "Python",
      "proficiency_level": "Expert"
    }
  ]
}
```
