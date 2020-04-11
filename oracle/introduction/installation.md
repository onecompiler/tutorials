
## Installation on Windows

1. Download the required version and edition from [Oracle Downloads](https://www.oracle.com/database/technologies/oracle-database-software-downloads.html)
2. Extract the zip contents and run setup.exe
3. setup will walk you through the trivial process.


## Installation on Linux
1. Download the required version and edition from [Oracle Downloads](https://www.oracle.com/database/technologies/oracle-database-software-downloads.html)
2. login as root and Unzip files
    ```shell
    unzip linux.x64_11gR2_database_1of2.zip
    unzip linux.x64_11gR2_database_2of2.zip
    ```
3. Using `yum`:
If you plan to use the "oracle-validated" package to perform all your required prerequisite setup, follow the instructions [here](http://public-yum.oracle.com) to setup the yum repository for OL, then perform the following command.
    ```shell
    yum install oracle-validated
    ```
