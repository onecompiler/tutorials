You can interact with files present outside the R environment. R can read and write into various file formats like csv, excel, json, xml, binary etc.

# CSV file

csv file is a text file where the column values are seperated by comma(,). 

## How to read a csv file

Consider `sample.csv` is present in the current working directory. 

```java
inp <- read.csv("sample.csv")
print(inp)
```

## How to write to a csv file

```java
write.csv(data,"filename.csv")
```

# Excel file

R interacts with excel file using `xlsx package`. 

Execute the below command to install xlsx package:

```java
install.packages("xlsx")
```
To verify the xlsx package installed by using the below command:

```java
any (grepl ("xlsx", installed.packages()))
```
## How to read a excel file

```java
inp <- read.xlsx("sample.xlsx", sheetIndex = 1)
print(inp)
```

# Binary File

Binary file stores data in the form of 0's and 1's. 

## How to read a Binary file
```java
readBin (conn, what, n )
```
## How to write to a binary file
```java
writeBin (object, conn)
```

* **conn** : connection object to read or write the binary file.
* **object**:  Object is the binary file to be written.
* **what**:  what is the mode like character, integer etc. representing the bytes to be read.
* **n**: number of bytes to read from the given binary file.

# XML file

R interacts with XML file using `xml package`. 

Execute the below command to install xlsx package:

```java
install.packages("XML")
```
## How to read a XML file

`xmlParse()` is used to read XML file in R.

```java
data <- xmlParse(file = "sample.xml")
print(data)
```
# MySQL Database

Execute the below command to install MySQL package.

```java
install.packages("RMySQL")
```

## Connecting to MySQL database

```java
mysqlconn = dbConnect(MySQL(), user = 'root', password = '', dbname = 'dbname',
   host = 'localhost')
```
## Querying database

```java
dbSendQuery(mysqlconn, "database query")
```
you can use select/update/insert/delete queries in the above command.
