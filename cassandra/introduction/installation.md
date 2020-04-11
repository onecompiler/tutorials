## Installing on MacOS

Easiest way to get Cassandra installed on MacOS is via Homebrew. You can run the following command to install Cassandra in MacOS
```sh
brew install cassandra
```

## Installing on Windows

We can download the latest version of cassandra from the following website
http://cassandra.apache.org/download/

Run the installation and open cassandra

### Installing Fron Debian Packages
```sh
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt-get update
sudo apt-get install cassandra
```

### Installing Fron RPM Packages
```sh
[cassandra]
name=Apache Cassandra
baseurl=https://www.apache.org/dist/cassandra/redhat/311x/
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.apache.org/dist/cassandra/KEYS

sudo yum install cassandra
```