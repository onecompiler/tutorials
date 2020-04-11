This query is used to select a single database from available set. all the following SQLite3 statements will be executed under the attached database.

```sh
ATTACH DATABASE 'DatabaseName' as 'Alias'
```

This command is used to list the databases created.
```sh
.databases
```

Database files are created in root folder.