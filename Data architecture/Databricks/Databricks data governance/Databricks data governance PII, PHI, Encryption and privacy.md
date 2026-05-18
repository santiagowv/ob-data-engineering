---
GDPR compliance: https://docs.databricks.com/aws/en/security/privacy/gdpr-delta
---

![[Pasted image 20260516191005.png|561]]
# Agentic data classification
Agentic AI system to automatically <span style="color:rgb(216, 203, 251)">discover and tag senstivie data across all catalogs</span>.
- **Audit-readiness:** pull <span style="color:rgb(216, 203, 251)">complete logs to show where PII resides</span> and exactly which users and groups have access to it.
- **Full lineage:** <span style="color:rgb(216, 203, 251)">trace exactly when PII exists</span> and where it flows downstream.
- **Data deletion requests:** <span style="color:rgb(216, 203, 251)">locate and clean up all instances of user data</span> across all tables.
- up to 60% higher accuracy than regex-only tools.
![[Pasted image 20260516182227.png|549]]
# Shared responsability of HIPAA compliance
- <span style="color:rgb(216, 203, 251)">Do not direct Databricks to send unencrypted PHI</span> to external services.
- <span style="color:rgb(216, 203, 251)">Ensure all data that may contain PHI is encrypted</span> at rest in any storage location the Databricks platform interacts with.
- Ensure <span style="color:rgb(216, 203, 251)">all data that may contain PHI is encrypted in transit between Databricks and any connected data storage</span> or external system.
## Enable enhanced security and compliance settings
Extra security controls for the workspace's compute environment. It is <span style="color:rgb(216, 203, 251)">not mainly a unity catalog permission feature</span>. I can include:
1. Enhanced security monitoring.
2. Compliance security profile.
3. Automatic cluster update.
4. One or more compliance standards, such as HIPAA, PCI-DSS, HITRUST, IRAP, FedRAMP.
# Right to be forgotten
![[Pasted image 20260516194818.png|578]]
- Start by <span style="color:rgb(216, 203, 251)">deleting data in the bronze layer first</span>, driven by a scheduled job that queries a table of deletion requests.
- After data is deleted from the bronze layer, <span style="color:rgb(216, 203, 251)">changes can be propagated to silver and gold layers</span>.
- We should regularly <span style="color:rgb(216, 203, 251)">maintain datasets to remove previous versions of data</span>. The recommended way is predictive optimization for unity catalog managed tables.
	- If we're not using LSDP, we should run a `VACUUM` command on Delta tables.
## Materialized views automatically handle deletions
![[Pasted image 20260516195835.png|577]]
It always <span style="color:rgb(216, 203, 251)">returns the correct result because it uses incremental computation</span> if it is cheaper than full recomputation.
## Delete data and read streaming source usin SkipChangeCommits
Streaming tables process <span style="color:rgb(216, 203, 251)">append-only data when they stream from Delta table</span> sources.
![[Pasted image 20260516200034.png]]
# PII data handling
## Classify
### Column tagging 
- Add the <span style="color:rgb(216, 203, 251)">right tags to columns like pii true/false, pii_type and classification</span>.
- We can query the `column_tags` table from the `information_schema` schema to see all applied tags.
```sql
ALTER TABLE pii_demo.pii_target.customers
ALTER COLUMN city
SET TAGS (
	'pii' = 'true',
	'pii_type' = 'indirect',
	'classification' = 'location'
)
```
## Protect
### Pseudonymization
Protects data at row level.
```python
from pyspark.sql import functions as F

df_hashed = df_source.select(
	F.sha2(F.col("full_name"), 256).alias("full_name"),
	F.md5(F.col("email")).alias("email")
)
```
### Tokenization
```python
# Create a UDF to generate unique tokens (UUIDs)
def generate_token():
	return str(uuid.uuid4())

generate_token_udf = F.udf(generate_token)

# Get distinct emails and generate tokens for each
email_lookup_df = source_df.select("email").distinct() \
	.withColumn("token", generate_token_udf())
	
print(f"Unique emails: {email_lookup_df.count()}")
```
### Categorical generalization
```python
df = spark.table("pii_demo.pii_source.employees")

# Define categorical generalization mapping for job titles
# Specific job titles will be grouped into broader categories
job_category_mapping = {
	"Software Engineer": "Engineering",
	"Data Scientist": "Data & Analytics",
	"Product Manager": "Management",
	"Sales Executive": "Sales & Business",
	"HR Manager": "Management"
}

# Create a mapping expression using CASE WHEN
mapping_expr = F.when(F.col("job_title") == "Software Engineer", "Engineering") \
	.when(F.col("job_title") == "Data Scientist", "Data & Analytics") \
	.when(F.col("job_title") == "Product Manager", "Management") \
	.when(F.col("job_title") == "Sales Executive", "Sales & Business") \
	.when(F.col("job_title") == "HR Manger", "Management") \
	.otherwise("other")
	
# Apply categorical generalization
df_generalized = df.withColumn("job_title_generalized", mapping_expr) \
	.drop("job_title") \
	.withColumnRenamed("job_title_generalized", "job_title")
	
# Write to target table
df_generalized.write.mode("overwrite").saveAsTable("pii_demo.pii_target.employees_categorical_generalized")
```
### Binning
```python
# Calculate age from date_of_birth
df_with_age = df.withColumn(
	"age",
	floor(months_between(current_date(), col("date_of_birth")) / 12)
)

# Apply binning to create age ranges
df_binned = df_with_age.withColumn(
	"age_range",
	when(col("age") < 18, "Under 18")
	.when((col("age") >= 18) & (col("age") <= 25), "18-25")
	.when((col("age") >= 26) & (col("age") <= 35), "26-35")
	.when((col("age") >= 36) & (col("age") <= 45), "36-45")
	.when((col("age") >= 46) & (col("age") <= 55), "46-55")
	.when((col("age") >= 56) & (col("age") <= 65), "56-65")
	.otherwise("66+")
)
```
### Truncating IP addresses
```python
from pyspark.sql.functions import col, regexp_replace

# Truncate IP address by replacing the last octet with 0
# Pattern matches the last octet (last number after the final dot)
df_truncated = df_web_logs.withColumn(
	"ip_address",
	regexp_replace(col("ip_address"), r"\d+$", "0")
)
```
### Rounding
```python
from pyspark.sql.functions import col, floor, round as spark_round

# Apply generalization by rounding salary to nearest 10.000
# This protects individual privacy while maintaining data utility
df_rounded = df_employees.withColumn(
	"salary_rounded",
	(floor(col("salary") / 10000) * 10000).cast("bigint")
)
```
### Encryption
```python
from pyspark.sql import functions as F

encryption_key = "MySecureKey12345"

df_encrypted = df_source.select(
	# Non-PII column
	F.col("customer_id"),
	
	# PPI columns - apply AES encryption and encode to base64 for readability
	F.base64(F.aes_encrypt(F.col("full_name"), F.lit(encryption_key))).alias("full_name")
)
```