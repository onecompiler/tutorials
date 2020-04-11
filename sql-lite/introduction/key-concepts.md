## Zero configuration
SQLite works without any installation, setup. There is no server which needs to started, stopped or configured. It required no configuration files. No actions are required to recover after a system crash.

## Serverless
Most of other database are implemented as a separate server. Users who wants to communicate with the server needs to send requests to the server and receive the results. With sqlite, the process that wants to access the database read and writes directly from the database file on to disk.

## Single Database File
SQLite database is a single ordinary disk file that can be located in the directory hierarchy. Database files can be easily transferred to USB memory stick or email.
