
## ISQL commands
 

| Command Syntax | Description|
|-----|-----|
|`BLOBDUMP <blobid> <file>`|dump BLOB to a file|
|`BLOBVIEW <blobid>`| view BLOB in text editor|
|`EDIT     [<filename>]` |edit SQL script file and execute|
|`EDIT`                  |edit current command buffer and execute|
|`HELP `                 |display this menu|
|`INput    <filename>`   |take input from the named SQL file|
|`OUTput   [<filename>]` |write output to named file|
|`OUTput              `  |return output to stdout|
|`SET      <option>  `   |(Use HELP SET for complete list)
|`SHELL    <command>`    |execute Operating System command in sub-shell|
|`SHOW     <object> [<name>]` | display system information where <object> = CHECK, COLLATION, DATABASE, DOMAIN, EXCEPTION, FILTER, FUNCTION,GENERATOR, GRANT, INDEX, PROCEDURE, ROLE, SQL DIALECT, SYSTEM, TABLE, TRIGGER, VERSION, USERS, VIEW|
|`EXIT `                 |exit and commit changes
|`QUIT`                  |exit and roll back changes|
