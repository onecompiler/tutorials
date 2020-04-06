Filehandle is used to open or close a file in Perl.

## How to Open a file?

```perl
open(filehandle,mode,filename)
```
* filehandle is a variable which associates with the file
* Below are the modes available in Perl

|Mode|Symbol|
|----|----|
|read|	< |
|write|	> |
|append| >>|

* Filename is the name of the file you want to open along with filepath.

### Example

Consider you want to read `sample.txt` file:

```perl
open(FH, '<', 'c:\sample.txt');
```

## How to close a file?

You must close the file after finishing read or write operations on the file. `close()` function is used to close a file.

```perl
close(FH);
```

## File test Operators

Below are some of the frequently used test operators which helps in checking about the file before performing read or write operations:

|File test Operator| Description|
|----|----|
|-r| checks if the file is readable|
|-w| checks if the file is writable|
|-x| checks if the file is executable|
|-o| checks if the file is owned by effective uid.|
|-T| checks if the file is an ASCII text file.|
|-B| checks if the file is a binary file.|
|-e| checks if the file exists.|
|-z| checks if the file is empty.|
|-s| checks if the file has nonzero size.|
|-f| checks if the file is a plain file.|
|-d| checks if the file is a directory.|
|-l| checks if the file is a symbolic link.|
|-p| checks if the file is a named pipe (FIFO.|
|-S| checks if the file is a socket.|
|-b| checks if the file is a block special file.|
|-c| checks if the file is a character special file.|

### Example

```perl
my $filename = 'c:\sample.txt';
if(-e $filename && -r _){
   print("$filename is present and readable \n");
}else{
   print("$filename is neither present nor readable\n");
}
```
