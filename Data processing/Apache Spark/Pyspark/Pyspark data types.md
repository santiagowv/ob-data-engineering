# Converting to Spark Types
This function <span style="color:rgb(216, 203, 251)">converts a type in another language to its corresponding Spark representation</span>.
```python
from pyspark.sql.functions import lit

df.select(lit(5), lit("five"), lit(5.0))
```
# Spark data types

| Data type     | Value type in Python                                                                                                                | API to access or create a data type                                                                          |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| ByteType      | `int` or `long`. Ensure that numbers are within the range of –128 to 127.                                                           | `ByteType()`                                                                                                 |
| ShortType     | `int` or `long`. Ensure that numbers are within the range of –32768 to 32767.                                                       | `ShortType()`                                                                                                |
| IntegerType   | `int`  or `long`.Numbers that are too large will be rejected by Spark SQL. It's best practice to use `LongType` .                   | IntegerType()                                                                                                |
| LongType      | `long` Ensure that numbers are within the range of –9223372036854775808 to 9223372036854775807. Otherwise, convert data to decimal. | `LongType()`                                                                                                 |
| FloatType     | `float` Numbers will be converted to 4-byte singleprecision floating-point numbers at runtime.                                      | `FloatType()`                                                                                                |
| DoubleType    | `float`                                                                                                                             | `DoubleType()`                                                                                               |
| DecimalType   | `decimal.Decimal`                                                                                                                   | `DecimalType()`                                                                                              |
| StringType    | `string`                                                                                                                            | `StringType()`                                                                                               |
| BinaryType    | `bytearray`                                                                                                                         | `BinaryType()`                                                                                               |
| BooleanType   | `bool`                                                                                                                              | `BooleanType()`                                                                                              |
| TimestampType | `datetime.datetime`                                                                                                                 | `TimestampType()`                                                                                            |
| DateType      | `datetime.date`                                                                                                                     | `DateType()`                                                                                                 |
| ArrayType     | `list`, `tuple`, or `array`                                                                                                         | ArrayType(elementType, [containsNull]). Note: The default value of containsNull is True.                     |
| MapType       | `dict`                                                                                                                              | MapType(keyType, valueType, [valueContainsNull]). Note: The default value of valueContainsNull is True.      |
| StructType    | `list` or `tuple`                                                                                                                   | StructType(fields). Note: fields is a list of StructFields. Also, fields with the same name are not allowed. |
| StructField   | The value type in Python of the data type of this field (for example, `Int` for a `StructField` with the data type `IntegerType`)   | StructField(name, dataType, [nullable]) Note: The default value of nullable is True.                         |
