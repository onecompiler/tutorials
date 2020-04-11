
## Installation on Windows
1. Download the required version of DB2 software from [IBM](https://www.ibm.com/analytics/db2?mhsrc=ibmsearch_a&mhq=db2#1833846)
2. Extract the product installation files and run setup.exe as an admin
3. Setup will guide you through the installation and any settings if required.

## Installation on Linux
### Using db2setup(GUI)
1. Download the required version of DB2 software from [IBM](https://www.ibm.com/analytics/db2?mhsrc=ibmsearch_a&mhq=db2#1833846)
2. Untar the zip file
```shell
tar -xvf DB2_downloaded_filename
```
3. set the DISPLAY variable to run the GUI installation setup.
```shell
EXPORT DISPLAY=:0.0
```
4. Go to Server directory and run the db2setup file
```shell
./db2setup
```
5. Now the Setup will guide you through the installation and any settings if required.
