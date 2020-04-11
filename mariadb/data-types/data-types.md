# String data types

| Data Type |Description| Maximum Size Limit |
| --------- | ----------|--------------------|
|CHAR(length)|Fixed length string, length specifies the number of characters to be stored|255|
|VARCHAR(length)|Variable length string,length specifies the number of characters to be stored|255|
|text(length)|Fixed length string, length specifies the number of characters to be stored|255|
|binary(length)|Fixed length string, length specifies the number of characters to be stored|255| 
| BLOB, TEXT |BLOB is for storing binary data and Text is used to store large string| 65,535(64KB) |
| TINYTEXT | used to store short strings |255 |
| MEDIUMTEXT| used to store larger text strings| 16,777,215(16MB) |
| LONG TEXT |used to store very large data which can't be stored in MEDIUMTEXT |4,294,967,295(4GB) |


# Data and time data types


| Data type | Format | Range |
| ---------- | ------ | ----- |
| DATE | 'YYYY-MM-DD' | '1000-01-01' to '9999-12-31' |
| DATETIME[(f)] | 'YYYY-MM-DD hh:mm:ss[.fraction]' | '1000-01-01 00:00:00.000000' to '9999-12-31 23:59:59.999999' | 
| TIMESTAMP[(f)] | 'YYYY-MM-DD hh:mm:ss[.fraction]' | '1970-01-01 00:00:01.000000' UTC to '2038-01-19 03:14:07.999999' UTC | 
| TIME[(f)] | 'hh:mm:ss[.fraction]' | '-838:59:59.000000' to '838:59:59.000000' | 
| YEAR[(2/4)] | 'YY'/'YYYY'(default) | Year represented in 2 or 4 digits|


Note: f refers to fractional seconds precision



# Numeric data types

### 1. Integer Family

| Data Type |	Unsigned Min Value | Unsigned Max Value | Signed 	Min Value | Signed	Max Value |	
| --------- | -------------------- | ------------------ | ----------------- | ----------------- |
| INT	|0	| 4294967295 |	-2147483648 |	2147483647 |
| SMALLINT  | 0	| 65535	| -32768 |	32767|
| MEDIUMINT	| 0	| 16777215 |	-8388608 |	8388607 |
| BIGINT	| 0	| 18446744073709551615| 	-9223372036854775808 |	9223372036854775807 |
| TINYINT	| 0	| 255 |	-128 |	127 |

### 2. FLOAT(d)
where d is number of decimals. If the d value is in between  0-24 then the data type becomes FLOAT() else if the d value is >24 and <53 then the  data type becomes DOUBLE()

### 3. DECIMAL(l,d)
where l is number of digits displayed before the decimal point and d is number of digits after the decimal point. Default value is DECIMAL(10,2).


