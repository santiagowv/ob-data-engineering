# COPY INTO
<span style="color:rgb(216, 203, 251)">Incrementally loads data</span> into Delta Lake tables from Cloud Storage.
- Supports schema evolution.
- <span style="color:rgb(216, 203, 251)">Supports wide range of file formats</span> (CSV, JSON, Parquet, Delta).
- Alternative to Auto Loader for batch ingestion.
## Create the table to copy the data into
```sql
CREATE TABLE IF NOT EXISTS demo.delta_lake.raw_stock_prices;
```
## Incrementally load new files into the table
```sql
COPY INTO demo.delta_lake.raw_stock_prices
FROM 'abfss://demo@deacourseextdl202617.dfs.core.windows.net/landing/stock_prices'
FILEFORMAT = CSV
FORMAT_OPTIONS ('ingerSchema' = 'true')
COPY_OPTIONS ('mergeSchema' = 'true');
```
# Merge
Used for <span style="color:rgb(216, 203, 251)">upserts</span> (insert/update/delete operations).
- Allows <span style="color:rgb(216, 203, 251)">merging new data into a target table</span> based on matching condition.
## Create the table to merge the data into
```sql
CREATE TABLE IF NOT EXISTS demo.delta_lake.stock_prices
(
  stock_id STRING,
  price DOUBLE,
  trading_date DATE
)
```
## Merge statement
```sql
MERGE INTO demo.delta_lake.stock_prices AS target
USING demo.delta_lake.raw_stock_prices AS source
  ON target.stock_id = source.stock_id
WHEN MATCHED AND source.status = "ACTIVE" THEN
  UPDATE SET target.price = source.price, target.trading_date = source.trading_date
WHEN MATCHED AND source.status = "DELISTED" THEN
  DELETE
WHEN NOT MATCHED AND source.status = "ACTIVE" THEN
  INSERT (stock_id, price, trading_date) VALUES (source.stock_id, source.price, source.trading_date);
```