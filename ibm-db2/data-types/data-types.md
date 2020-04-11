# String data types

| Data Type |Description| Maximum Size Limit |
| --------- | ----------|--------------------|
|CHARACTER(length)|Fixed length string, length specifies the number of characters to be stored|255|
|VARCHAR(length)|Variable length string,length specifies the number of characters to be stored|255|
|CLOB(length)|Variable length string,length specifies the number of characters to be stored|2,147,483,647|
|GRAPHIC(length)|Fixed-length graphic strings, length specifies the number of double-byte characters to be stored|128|
|VARGRAPHIC(length)|Variable length string,length specifies the number of double-byte characters to be stored|16352| 
| DBCLOB(length) |Variable length string,length specifies the number of double-byte characters to be stored | 1,073,741,824 |
|BINARY(length)|Fixed-length or variable-length binary strings , length specifies the number of binary characters to be stored|255|
|VARBINARY(length)|Variable binary string,length specifies the number of binary characters to be stored|32704|
| BLOB(length) | Variable length string, length specifies the number of bytes to be stored  |2,147,483,647 |



# Data and time data types


| Data type | Format | Range |
| ---------- | ------ | ----- |
| DATE | 'YYYY-MM-DD' | 0001-01-01 to 9999-12-31 |
| TIMESTAMP | 'YYYY-MM-DD-hh.mm.ss.fraction' | 0001-01-01-00.00.00.000000000 to 9999-12-31-24.00.00.000000000 | 
| TIME | 'hh:mm:ss' |00.00.00 to 24.00.00 | 




# Numeric data types


| Data type | Description |
| ---------- | ------ | 
|SMALLINT| Small Integer with range of -32768 to +32767|
|INT| Large Integer with range of  -2147483648 to +2147483647|
|BIGINT| A very large Integer with range of  range is -2147483648 to +2147483647|
|DECIMAL or NUMERIC| A packed decimal number with range of 1 - 10³¹ to 10³¹ - 1|
|DECFLOAT|A decimal floating-point number with range is either 16 or 34 digits of precision|
|REAL|This is a short floating-point number of 32 bits with range  approximately -7.2E+75 to 7.2E+75|
|DOUBLE|This is a long floating-point number of 64 bits with range  approximately -7.2E+75 to 7.2E+75 |

# XML Data type

The XML data type is used to define columns of a table can store XML values.
The size of an XML value has no architectural limit but serialized XML data that is stored in or retrieved from an XML column is limited to 2 GB.