## Installation on Linux

1. Download the required version from [EnterpriseDB](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
2. Login as root and execute below commands
    ```
    [root@host]# chmod +x postgresql-10.10-1-linux-x64.run
    [root@host]# ./postgresql-10.10-1-linux-x64.run
    ```
3. Provide location, port number, password of the username etc., to complete the installation. 

## Installation on Windows

1. Download the required version from [EnterpriseDB](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
![](images/postgres-installation-on-windows-1.png)
2. Run the executable file and specify the installation directory.
![](images/postgres-installation-on-windows-2.png)
3. Select the components as per your requirement. Stack Builder is used to download additional components for future purpose.
![](images/postgres-installation-on-windows-3.png)
4. Provide the password.
![](images/postgres-installation-on-windows-4.png)
5. provide port number
![](images/postgres-installation-on-windows-5.png)
6. Setup is ready now to start the installation process.
![](images/postgres-installation-on-windows-6.png)
7. Installation takes few minutes to complete.
![](images/postgres-installation-on-windows-7.png)
8. Verify the installation.
    * Open psql
    * Provide servername, database name, port number, user name and password which you have provided during installation.
    * Check the version using 
        ```sql
        select version();
        ```
        ![](images/postgres-installation-on-windows-8.png)

## Connect to database

1. open pgadmin 4
2. provide password
3. If server is not already available, create by right clicking on Servers and provide hostname, username and passsword.
    ![](images/postgres-installation-on-windows-9.png)
4. Open Querytool by right clicking on the database you created.
    ![](images/postgres-installation-on-windows-10.png)
