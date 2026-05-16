# EL
- Only for data that is already clean and correct.
- Scheduled periodic loads of log files (e.g. once a day).
- Batch load of historical data.
# ELT
- Experimental datasets where we are not yet sure what kinds of transformations are needed to make data useable.
- Any production dataset where the transformation can be expressed in SQL.
# ETL
- When the <span style="color:rgb(216, 203, 251)">raw data needs to be quality-controlled, transformed, or enriched before being loaded</span> into BigQuery.
- When <span style="color:rgb(216, 203, 251)">the data loading has to happen continuously</span>, i.e if the use case requires streaming.
- When you want to integrate with continuous integration/continuous delivery (CI/CD) systems and perform unit testing on all components.
