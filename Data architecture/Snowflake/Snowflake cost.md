# Compute cost
- It's measured in terms of <span style="color:rgb(216, 203, 251)">Snowflake credits consumed and not dollars</span>.
- A Snowflake Credit is a <span style="color:rgb(216, 203, 251)">unit of measure to quantify the amount of compute resources consumed in Snowflake</span>.
- <span style="color:rgb(216, 203, 251)">Price of a Snowflake Credit changes depending on the edition we use</span>. The higher the version the greater the cost per credit.
# Storage cost
## On-demand storage
## Capacity storage
- Account is charged for the storage is used.
- Cost per TB is $40 per month.
## Capacity storage
- Storage is pre-provisioned upfront and account <span style="color:rgb(216, 203, 251)">pays for the capacity we provisioned irrespective of how much data we use</span>.
- Cost per TB is $23 per month.
# Data transfer cost
- Snowflake applies data transfer cost if <span style="color:rgb(216, 203, 251)">data stored in Snwoflake is transferred out of Snowflake</span>.
- The process of transferring data out of Snowflake is <span style="color:rgb(216, 203, 251)">also called data egress</span>.
- Data transfer cost or <span style="color:rgb(216, 203, 251)">data egress costs vary based on regions</span>.
- Data ingress is generally free.
- <span style="color:rgb(216, 203, 251)">Egress cost is between $90 to $150</span> per TB from Swowflake to external systems.
# Cloud services cost
- The cloud services compute usage is <span style="color:rgb(216, 203, 251)">only charged if it exceeds 10% of the daily compute</span> usage on the account.