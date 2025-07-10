# Installing Firebird

This guide covers installing Firebird 4.0 (the latest stable version) on various operating systems. Firebird offers different server architectures, so choose the one that best fits your needs.

## Server Architecture Options

Before installing, understand the available architectures:

- **SuperServer**: Best for many concurrent users with shared cache
- **Classic**: Process-per-connection model, good for few heavy users
- **SuperClassic**: Hybrid approach with thread-per-connection
- **Embedded**: In-process, serverless deployment

## Installation on Windows

### Using the Installer (Recommended)

1. Download the latest Firebird 4.0 installer from [firebirdsql.org](https://firebirdsql.org/en/downloads/)
   - Choose 64-bit or 32-bit based on your system
   - File format: `Firebird-4.0.x.xxxxx-0_x64.exe`

2. Run the installer as Administrator:
   - Right-click the installer → "Run as administrator"
   
3. Follow the installation wizard:
   - Accept the license agreement
   - Choose installation directory (default: `C:\Program Files\Firebird\Firebird_4_0`)
   - Select components (Server, Client, Developer files)
   - Choose server architecture (SuperServer recommended for most users)
   - Set the SYSDBA password (remember this!)
   - Choose to run as a service (recommended)

4. Verify installation:
   ```cmd
   cd "C:\Program Files\Firebird\Firebird_4_0\bin"
   isql -user SYSDBA -password yourpassword
   SQL> CREATE DATABASE 'C:\temp\test.fdb';
   SQL> QUIT;
   ```

### Using ZIP Archive

1. Download the ZIP archive from the official website
2. Extract to your desired location
3. Run `install_service.bat` as Administrator
4. Configure `firebird.conf` as needed

## Installation on Linux

### Ubuntu/Debian (Using APT)

```bash
# Update package list
sudo apt update

# Install Firebird 4.0
sudo apt install firebird3.0-server

# Note: Package name may still be firebird3.0 in some repositories
# For latest version, use official packages
```

### Using Official Packages (Recommended)

1. Download the appropriate package:
   ```bash
   # For Ubuntu/Debian (64-bit)
   wget https://github.com/FirebirdSQL/firebird/releases/download/v4.0.2/Firebird-4.0.2.2816-0.amd64.tar.gz
   
   # Extract the archive
   tar -xzf Firebird-4.0.2.2816-0.amd64.tar.gz
   cd Firebird-4.0.2.2816-0.amd64
   ```

2. Run the installation script:
   ```bash
   sudo ./install.sh
   ```

3. During installation:
   - Press Enter to start installation
   - Enter new SYSDBA password when prompted
   - Choose to start Firebird at boot (recommended)

4. Verify installation:
   ```bash
   # Check service status
   sudo systemctl status firebird-superserver
   
   # Test connection
   /opt/firebird/bin/isql -user SYSDBA -password yourpassword
   SQL> CREATE DATABASE '/tmp/test.fdb';
   SQL> QUIT;
   ```

### CentOS/RHEL/Fedora

```bash
# Download RPM package
wget https://github.com/FirebirdSQL/firebird/releases/download/v4.0.2/Firebird-4.0.2.2816-0.x86_64.rpm

# Install
sudo rpm -ivh Firebird-4.0.2.2816-0.x86_64.rpm

# Or using yum/dnf
sudo yum localinstall Firebird-4.0.2.2816-0.x86_64.rpm
```

## Installation on macOS

### Using Homebrew

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Firebird
brew install firebird

# Start Firebird service
brew services start firebird

# Verify installation
isql -user SYSDBA -password masterkey
```

### Manual Installation

1. Download the macOS package from [firebirdsql.org](https://firebirdsql.org)
2. Open the `.pkg` file and follow the installer
3. Add Firebird to your PATH:
   ```bash
   echo 'export PATH="/Library/Frameworks/Firebird.framework/Resources/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```

## Post-Installation Configuration

### 1. Change SYSDBA Password

```bash
# Connect to security database
isql -user SYSDBA -password masterkey

# Change password
SQL> ALTER USER SYSDBA SET PASSWORD 'NewSecurePassword';
SQL> QUIT;
```

### 2. Configure firebird.conf

Location varies by platform:
- Windows: `C:\Program Files\Firebird\Firebird_4_0\firebird.conf`
- Linux: `/opt/firebird/firebird.conf`
- macOS: `/Library/Frameworks/Firebird.framework/Resources/firebird.conf`

Common settings to adjust:
```conf
# Database location restrictions
DatabaseAccess = Full

# Default database location
DefaultDbCachePages = 4096

# Network settings
RemoteServicePort = 3050

# Authentication
AuthServer = Srp256, Win_Sspi, Legacy_Auth
AuthClient = Srp256, Win_Sspi, Legacy_Auth

# Wire encryption
WireCrypt = Enabled
```

### 3. Create Your First Database

```bash
# Using isql
isql -user SYSDBA -password yourpassword

SQL> CREATE DATABASE 'localhost:/var/firebird/mydatabase.fdb'
CON> USER 'SYSDBA' PASSWORD 'yourpassword'
CON> PAGE_SIZE 16384
CON> DEFAULT CHARACTER SET UTF8;

SQL> QUIT;
```

## Installing GUI Tools

### FlameRobin (Cross-platform)

1. Download from [flamerobin.org](http://www.flamerobin.org)
2. Install for your platform
3. Create a new server registration:
   - Server: localhost
   - Port: 3050
   - Username: SYSDBA
   - Password: your password

### DBeaver (Universal Database Tool)

1. Download from [dbeaver.io](https://dbeaver.io)
2. Install Firebird driver when prompted
3. Create new connection with Firebird type

## Verifying Installation

Test your installation with these commands:

```bash
# Check Firebird version
fbsvcmgr localhost:service_mgr user SYSDBA password yourpassword info_server_version

# Check server status
fbsvcmgr localhost:service_mgr user SYSDBA password yourpassword info_svr_db_info

# Create and connect to a test database
isql -user SYSDBA -password yourpassword
SQL> CREATE DATABASE 'test.fdb';
SQL> CONNECT 'test.fdb';
SQL> CREATE TABLE test_table (id INTEGER, name VARCHAR(50));
SQL> INSERT INTO test_table VALUES (1, 'Test');
SQL> SELECT * FROM test_table;
SQL> DROP DATABASE;
SQL> QUIT;
```

## Troubleshooting

### Common Issues

1. **Port 3050 already in use**
   - Check if another Firebird instance is running
   - Change port in `firebird.conf`

2. **Authentication errors**
   - Ensure correct SYSDBA password
   - Check AuthServer/AuthClient settings
   - For legacy applications, enable Legacy_Auth

3. **Cannot create database**
   - Check file permissions
   - Verify DatabaseAccess setting
   - Ensure sufficient disk space

4. **Service won't start**
   - Check firebird.log for errors
   - Verify no other instances running
   - Check file permissions on Firebird directory

## Next Steps

Now that Firebird is installed:
1. Learn basic SQL commands
2. Create your first database
3. Explore Firebird's unique features
4. Set up regular backups with gbak