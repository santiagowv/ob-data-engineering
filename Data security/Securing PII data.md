Name + Email = PII
Name + Medical diagnosis = PHI
# PII
Personal identifiable information. It's any data that can <span style="color:rgb(216, 203, 251)">directly or indirectly identify a specific individual</span>.
## Direct PII
- <span style="color:rgb(216, 203, 251)">Uniquely identifies a person</span> on its own.
- Examples are full name, email address, phone number, SSN, passport number.
## Indirect PII
- Identifies a person when <span style="color:rgb(216, 203, 251)">combined with other data</span>.
- Examples are Date of birth, IP address, device id.
# Process of PII data handling
1. **Minimize:** only collect and process PII that is <span style="color:rgb(216, 203, 251)">absolutely necessary</span>.
2. **Classify:** identify and <span style="color:rgb(216, 203, 251)">tag PII as early as ingestion</span>.
3. **Protect:** encrypt, pseudonymize or <span style="color:rgb(216, 203, 251)">anonymize based on sensitivity</span>.
4. **Control:** enforce strict <span style="color:rgb(216, 203, 251)">access policies at column and row level</span>.
5. **Monitor:** continuously <span style="color:rgb(216, 203, 251)">audit who accessed what and when</span>.
6. **Delete:** support data <span style="color:rgb(216, 203, 251)">deletion and retention policies for compliance</span>.
## Minimize
Only collect and <span style="color:rgb(216, 203, 251)">process PII that is absolutely necessary</span>.
- Do not ingest the tables containing PII if no really required.
- <span style="color:rgb(216, 203, 251)">Exclude PII columns which are not needed</span> for business use case.
## Classify
Identify and tag PII as early as ingestion.
- Add the <span style="color:rgb(216, 203, 251)">right tags to columns like pii true/false, pii_type and classification</span>.
- Can use <span style="color:rgb(216, 203, 251)">inbuild data classification feature in databricks</span>.
## Protect
Encrypt, pseudonymize or <span style="color:rgb(216, 203, 251)">anonymize based on sensitivity</span>.
### Pseudonymization
<span style="color:rgb(216, 203, 251)">Replace original data</span> with different ids. <span style="color:rgb(216, 203, 251)">Protects data</span> at row level. Re identification may be possible.
- **Hashing:** <span style="color:rgb(216, 203, 251)">calculate hash values and replace data</span> with hash values.
	- Example: sha, md5 functions.
- **Tokenization:** <span style="color:rgb(216, 203, 251)">move PII columns to a separate look up table</span> along with custom keys.
	- Restrict access to look up table.
### Anonymization
<span style="color:rgb(216, 203, 251)">Replace original data with category</span>, summarized data etc. <span style="color:rgb(216, 203, 251)">Protects entire dataset</span>. Re identification is not possible.
- **Data supression:** <span style="color:rgb(216, 203, 251)">exclude PII columns</span> from views. Dynamic access control.
- **Generalization:**
	- **Categorical generalization:** <span style="color:rgb(216, 203, 251)">removal precision</span>. Move from specific category to more general.
	- **Binning:** replace data with groupings.
	- **Truncating IP addresses:** <span style="color:rgb(216, 203, 251)">generalize IP geolocation to city or higher level</span>. Replace last byte with 0.
	- **Rounding:** round the numbers to <span style="color:rgb(216, 203, 251)">remove precision</span>.
### Encryption
<span style="color:rgb(216, 203, 251)">Encrypt the data using keys</span>. Store encrypted data. Decrypt the data during retrieval. Re identification is possible.
- **Column encryption:** <span style="color:rgb(216, 203, 251)">Encrypt the column value using encryption key</span> before storing them in table. Values are reversible.
- **Encryption at rest:** Data stored in storage account is <span style="color:rgb(216, 203, 251)">always encrypted by default</span>. Can be encrypted using customer managed keys (CMK) as well.
- **Encryption in motion:** <span style="color:rgb(216, 203, 251)">Data is encrypted while in motion</span>. TLS/SSL for data in transit.
## Control
- Role based access control.
- Attribute based access control.
- Row filtering.
- Column masking.
## Monitor
- Detailed access logs.
- A monitoring dashboard.
## Delete
- All tables must support deletion of data.
- Vacuum must be run to actually delete the data.
# PHI
<span style="color:rgb(216, 203, 251)">Protected health information</span>, which is a subset of PII that specifically relates to an <span style="color:rgb(216, 203, 251)">individual's health history and/or status</span>.
- It's under HIPAA (Health insurance portability and accountability act) a US healthcare law that establishes <span style="color:rgb(216, 203, 251)">national standards for protecting the privacy and security of PHI</span>.
# Anonymizing decision making process
![[Pasted image 20260516193647.png|501]]
# GDPR compliance
General data protection regulation (GDPR) and California consumer privacy act (CCPA) are <span style="color:rgb(216, 203, 251)">privacy and data security regulations that require companies to delete all PII</span>.
- Deletion <span style="color:rgb(216, 203, 251)">requests must be executed during a specified period</span>.