This chapter explains you on how to handle files

## 1. How to open a file

`fopen()` function is used to open a file.

```php
fopen("filename", "mode");
```
|Mode | Description|
|----|----|
|r | read only|
|w | write only|
|a | Appends data to the end of the file |
|x | write only but returns FALSE if file already exists|
|r+| Opens a file for read/write.|
|w+| Open a file for read/write. Overwrites the contents|
|a+| Open a file for read/write. Appends data to the end of the file|
|x+| Creates a new file for read/write but returns FALSE if file already exists|

## 2. How to read a file

`fread()` function is used to read a file.

```php
fread($filename, filesize)
```
* **filename** : Name of the file to read
* **filesize** : Maximum number of bytes to read.


## 3. How to create a file

`fopen()` function is also used to create a file. Syntax and modes are given above.

## 4. How to write to a file

`fwrite()` function is used to write to a file.

```php
fwrite($filename, $data);
```

* **filename** : Name of the file to write data
* **data** : data to write to the file.

Note: You need to open the file before writing the data to it and close it after once you finish writing.











## 5. How to close a file

`fclose()` function is used to close a file.

```php
fclose($filename);
```
* **filename** : Name of the file to close
