DB2 instance is a logical environment for a IBM DB2 database Manager. A physical machine can contain multiple DB2 instances. A DB2 instance can contain multiple databases.

## DB2 instance directory contents:

* Database Manager Configuration file
* System Database Directory
* Node Directory
* Node Configuration File [db2nodes.cfg] and
* Debugging files, dump files

## How to create DB2 instance 
1. Login as root

2. create a seperate operating system user for the instance you want to create.
    ```sh
    useradd -u <ID> -g <group name> -m -d <user location> <user name> 
    -p <password>
    ```
3. Go to DB2 instance directory
    ```sh
    cd /opt/ibm/db2/v10.1/instance 
    ```
4. create DB2 instance
    ```sh
    ./db2icrt -s ese -u <inst id> <instance name>
    ```
5. Verify using list the instances present
    ```sh
    db2ilist 
    ```

## How to update a DB2 instance

 This command is used to update the DB2 instance with same version release. 

 Note: Stop the instance before updating it

### Syntax

```sh
#To update the instance in normal mode
db2iupdt <inst_name>

#To update the instance in debugging mode
db2iupdt -D <inst_name> 
```

## How to upgrade a DB2 instance

This command is used to upgrade the current version of DB2 instance to a higher version.
Navigate to DB2DIR/instance directory on UNIX/Linux.

### Syntax

```sh
db2iupgrade -d -k -u <inst_username> <inst_name>
```
Note:
```
-d: debbuging mode
-k: to keep the pre-upgrade version in the DB2copy if it's supported.
```


## How to delete a DB2 instance
This command is used to delete a DB2 instance. Navigate to DB2_installation_folder/instance directory on Unix/Linux.

### Syntax
```sh
db2idrop -u <inst_username> <inst_name> 
```

## Commands
1. Get Instance
 This command shows the name of the active DB2 instance.
    ### Syntax
    ```sh
    db2 get instance 
    ```
2. Set Instance
This command is issued to refer the instance name before start or stop of the database manager of a DB2 instance
    ### Syntax
    ```sh
    set db2instance=<instance_name>
    ```
3. Start a DB2 instance
This command is used to start a DB2 instance
    ### Syntax
    ```sh
    db2start
    ```
4. Stop a DB2 instance
This command is used to stop a DB2 instance
    ### Syntax
    ```sh
    db2stop

    ```
