PHP can interact with almost all the databases but most commonly used database is MySQL. Let's discuss more in detail on how PHP interacts with MySQL here.

## How to connect to MySQL database

You can connect with MySQL using MySQLi extension or PHO (PHP Data Objects). With PDO, you can work on 12 different database systems but with MySQLi, you can only work with MySQL database.

Usually MySQLi extension is automatically available with PHP, if not you can refer installation steps [here](https://www.php.net/manual/en/mysqli.installation.php).

Refer PDO installation steps [here](http://php.net/manual/en/pdo.installation.php)

## How to Open a MySQL database connection

MySQLi:
```php
$conn = new mysqli($servername, $username, $password);
```
PDO:
```php
$conn = new PDO("mysql:host=$servername;dbname=DBname", $username, $password);
```
## How to Close a MySQL database connection

MySQLi:
```php
$conn->close();
```
PDO:
```php
$conn = null;
```
# CRUD Operations 

##  How to Create a table

```php
<?php
$servername = "localhost"; //servername or localhost
$username = "userName";
$password = "password";
$db = "DatabaseName";

$conn = new mysqli($servername, $username, $password, $db); // creating connection to MySQL database

if ($conn->connect_error) {
  die("There is an error while connecting to database " . $conn->connect_error);
}

// creating a sample table
$sql = "CREATE TABLE STUDENT ( 
id INT(5) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
firstname VARCHAR(30) NOT NULL,
lastname VARCHAR(30) NOT NULL,
pursuing VARCHAR(30) 
)";

if ($conn->query($sql) === TRUE) {
  echo "Table is created successfully";
} else {
  echo "There is an error while creating table " . $conn->error;
}

$conn->close();
?>
```
## How to Insert a record into the table

Once you establish the connectivity to the database, you can execute insert query as below:

```php
$sql = "INSERT Query";

if ($conn->query($sql) === TRUE) {
  echo "one record is inserted successfully";
} else {
  echo "Error while inserting the record " . $conn->error;
}
```

## How to Insert multiple records

Once you establish the connectivity to the database, you can execute insert mutiple records as below:

```php
$sql = "INSERT Query1";
$sql .= "INSERT Query2";
$sql .= "INSERT Query3";

if ($conn->multi_query($sql) === TRUE) {
  echo "Records are inserted successfully";
} else {
  echo "Error while inserting record " . $conn->error;
}
```

## How to read records from a table

Once you establish the connectivity to the database, you can execute select query as below:

```php
$sql = "SELECT Query";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
  //code if records are fetched
} else {
  echo "0 records are found";
}
```
## How to Update a record

Once you establish the connectivity to the database, you can execute update query as below:

```php
$sql = "UPDATE Query";

if ($conn->query($sql) === TRUE) {
  echo "Record is updated successfully";
} else {
  echo "Error while updating record " . $conn->error;
}
```
## Delete records

Once you establish the connectivity to the database, you can execute delete query as below:

```php
$sql = "DELETE Query";

if ($conn->query($sql) === TRUE) {
  echo "Record is deleted";
} else {
  echo "Error while deleting the record " . $conn->error;
}
```
