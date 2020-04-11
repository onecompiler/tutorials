# Installation on MacOS

## Using Homebrew
1. Once you install Homebrew, execute the below command to install MariaDB
```shell
brew install mariadb
```
2. Start Maridb Server
```shell
mysql.server start
```

# Installation on Linux

* Download the required version from [MariaDB Downloads](https://downloads.mariadb.org/)
* MariaDB offers packages for RedHat/CentOS/Fedora and Debian/Ubuntu
* openSUSE, Arch Linux, Mageia, Mint, Slackware distributions already include MariaDB package in their repositories.
## How to install
1. Login as `root`
2. Navigate to MariaDB directory and import GnuPG signing key
    ```shell
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
    ```
4. Add MariaDB to sources.list file and add the below code in the file
    ```shell
    sudo add-apt-repository 'deb http://ftp.osuosl.org/pub/mariadb/repo/5.5/ubuntuprecise main'
    ```
5. Refresh the system using below command
    ```shell
    sudo apt-get update
    ```
6. Install MariaDB
    ```shell
    sudo apt-get install mariadb-server
    ```

# Installation on Windows

* Installation on windows is much easier using MSI file. Download the required version from [MariaDB Downloads](https://downloads.mariadb.org/)
* Just double click on the file and the setup will guide you through the installation and any settings if required.