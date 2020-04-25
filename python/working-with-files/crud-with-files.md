open() function is used to do CRUD(create,read,update,delete) operations on files in Python.

## Syntax of open function

```py
open(filename,mode)
```
* filename - specifies the name of the file
* mode - `c`(create), `r`(read),`a`(append), `w`(write)
* Along with the above mode options, you can also specify the operation mode as either `t`(text) or `b`(binary)

## Create

Use open() function with `c` or `a` or `w` as mode.

### Example
```py
file = open("myfile.txt","c")
```

## Read
Use open() function with `r` as mode.

### Example
```py
file = open("myfile.txt","r")
print(file.read())
```

## Update or Append
Use open() function with  `a` or `w` as mode.

### Example
```py
file = open("myfile.txt","a")
file.write("Happy learning!!")
file.close()
```

## Delete

For deleting files, you must import os module and use `os.remove()` function.

### Example
```py
import os
os.remove(filename)
```

