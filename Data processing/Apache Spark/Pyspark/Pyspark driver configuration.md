We control the Spark application through a <span style="color:rgb(216, 203, 251)">driver process called the SparkSession</span>.
- It's the way Spark <span style="color:rgb(216, 203, 251)">executes user-defined manipulations</span> across the cluster.
- <span style="color:rgb(216, 203, 251)">One-to-one correspondance</span> between a SparkSession and a Spark Application.
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Santiago's Spark App").getOrCreate()

my_range = spark.range(1000).toDF("number")
```