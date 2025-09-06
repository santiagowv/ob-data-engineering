![[Drawing 2025-08-24 10.52.42.excalidraw|600]]
Common approaches to building a data warehouse are:
- Inmon's approach.
- Kimball's approach.
# Data warehouse model types
## Enterprise
Corporate wide data integration from one or more operational systems or external data providers.
## Data mart
Subset of corporate wide data that is of value to a specific collection of users.
A data mart is divided into two parts:
- **Independent data mart:** the source data comes from one or more operational systems.
- **Dependent data mart:** the source data comes from exactly from enterprise data warehouses.
## Virtual warehouse
This is a set of perception over the operational databasefor effective query processing.