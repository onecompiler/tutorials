open() function is used to do CRUD(create,read,update,delete) operations on files in Python.

## Syntax of open function

```py
open(filename,mode)
```
* filename - specifies the name of the file
* mode - `r`(read), `w`(write), `a`(append), `x`(exclusive creation)
* Along with the above mode options, you can also specify the operation mode as either `t`(text) or `b`(binary)

## Create

Use open() function with `x` (exclusive creation - fails if file exists), `w` (write - creates if doesn't exist), or `a` (append - creates if doesn't exist) as mode.

### Example
```py
with open("myfile.txt","x") as file:
    file.write("File created!")
```

## Read
Use open() function with `r` as mode.

### Example
```py
with open("myfile.txt","r") as file:
    print(file.read())
```

## Update or Append
Use open() function with  `a` or `w` as mode.

### Example
```py
with open("myfile.txt","a") as file:
    file.write("Happy learning!!")
```

## Delete

For deleting files, you must import os module and use `os.remove()` function.

### Example
```py
import os
os.remove(filename)
```

