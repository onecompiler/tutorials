MongoDB is available as a cloud offering which is called MongoDB atlas, use can use it with out you maintaining it. But if you choose to Install and manage by your own you can do that. Following sections shows you how to install it in different platforms.

## Installing on MacOS

Easyes way to get MongoDB installed on MacOS is via Homebrew. You can run the following command to install MongoDB in MacOS
```sh
brew install mongodb
```

## Installing on Windows

MongoDB is provided as an MSI installer package for Windows, we can get it from the following link
https://www.mongodb.com/download-center/community
After downloading, we need to run the installer and install it on Windows. 

## Installing on Ubuntu
We can install MongoDB on Ubuntu by running the following series of commands


### For Ubuntu 16.04
```sh
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

### For Ubuntu 18.04
```sh
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```