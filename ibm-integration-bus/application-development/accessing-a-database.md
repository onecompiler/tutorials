Databases are used to store business data and you must connect to it from integration node by using ODBC or JDBC.

Below are the message nodes where you can access a database.

* Compute
* Database
* DatabaseInput
* DatabaseRetrieve
* DatabaseRoute
* Filter
* JavaCompute
* Mapping

## Accessing databases from message flows

1. Define a connection to Data source name (DSN).

    * Define JDBC connection, if you are using JavaCompute or mapping node.

    * Define ODBC connection, if you are interacting from ESQL.

2. Provide neccessary authorizations to integration node to access the database.


## Accessing databases from ESQL

1. Provide ODBC DSN name in the `Data Source` property of message node in the message flow

2. Provide neccessary authorizations to integration node to access the database.(should have admin privileges).

