# Installation on MacOS

1. Download the default 1.5.1 package
2. Unzip the contents into a temp directory
    ```sh
    $ cd temp
    ~/temp $ unzip Firebird-CS-1.5.1-MacOS.zip
    ```
3. Install the package using below command
    ```sh
    ~/temp $ open Firebird-CS-1.5.pkg
    ```
4. Add the below lines to .bash_profile file
    ```sh
    export FIREBIRD_HOME=/Library/Frameworks/Firebird.framework/Resources
    export PATH=$PATH:$FIREBIRD_HOME/bin
    ```

# Installing and Configuring Superclassic on Linux

## How to install
1. Download the required version from [Firebird Downloads](http://www.firebirdsql.org/en/firebird-2-5/) 
2. Add ppa to your system.
    ```she
    sudo add-apt-repository ppa:mapopa/ppa
    sudo apt-get update
    ```
3. Inspect firebird2.5 related packages
    ```sh
    apt-cache search firebird2.5-*
    ```
4. Install the super server package by providing SYSDBA package and select service among Super Server, Classic or Super Classic
    ```sh
    sudo apt-get install firebird2.5-super
    ```
5. or Install classic using below command
    ```sh
    sudo apt-get install firebird2.5-classic
    ```
6. or Install super-classic using below command
    ```sh
    sudo apt-get install firebird2.5-superclassic
    ```
7. Configure the package after the installation.
    ```sh
    sudo dpkg-reconfigure firebird2.5-super
    ```
8. Install examples and dev files
    ```sh
    sudo apt-get install firebird2.5-examples firebird2.5-dev
    ```

# Installation on Windows

* Installation on windows is much easier using exe file. Download the required version from [Firebird Downloads](https://firebirdsql.org/en/firebird-2-5/)
* Run the executable file as an administrator and the setup will guide you through the installation and any settings like password etc.