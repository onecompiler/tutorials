DELETE command is used to delete data from cassandra table, You can delete complete table or particular data based on condition

```sh
DELETE FROM <identifier> WHERE <condition>;   
```

### Deleting an Entire Table
```sh
DELETE FROM student;   
```

### Deleting an Particular Data based on condition
WHERE clause is specified.
```sh
DELETE FROM student where id = 3;   
```

### Deleting an specific column name
```sh
DELETE city FROM student WHERE id=4;   
```

