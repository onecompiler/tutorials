# Installation on Linux

* Download  Microsoft SQL Server 2017 Red Hat repository configuration file using the below command

    ```sh
    sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
    ```
* Install SQL Server

    ```sh
    sudo yum install -y mssql-server
    ```
* After Installation gets finshed, run mssql-conf setup and set the SA password, edition etc.

    ```sh
    sudo /opt/mssql/bin/mssql-conf setup
    ```
* Check whether the services are running using below command
    ```sh
    systemctl status mssql-server
    ```
* Open SQL server port for allowing remote connections(Default port:1433).
    ```sh
    sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
    sudo firewall-cmd --reload
    ```


# Installation on Windows

* Download the required version of Developer edition from [SQL Server Downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
* Run the executable file and the setup will walk you through the trivial process.
* Installation will take few minutes to complete.
