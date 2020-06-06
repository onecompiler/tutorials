class IO provides all the basic methods of file such as read, write, readline, getc, gets, puts and printf in Ruby.

## Create a file

`File.new` method is used to create a File object for reading, writing, or both by specifying the mode string. `File.close` method is used to close the file.

```ruby
fh= File.new("filename", "mode")
   #code
fh.close
```

* filename - specifies the name of the file
* mode options are as follows :

 | Mode | Description|
 |----|----|
 |"r"  |  Read-only(default mode)|
 |"r+" |  Read-write |
 |"w"  |  Write-only |
 |"w+" |  Read-write |
 |"a"  |  Write-only, appends data at the end of file if file exists else creates a new file|
 |"a+" |  Read-write |
 |"b"  |  Binary file mode |
 |"t"  |  Text file mode |

## Opening a file

`File.open` method is same as `File.new` method with only one difference that you can associate a block of code to `File.open` method.

```ruby
File.open("filename", "mode") do |aFile|
   #code
end
```
## Read a file

```ruby
fh = File.new("sample.txt", "r")
if fh
#code
else
# Unable to open file
end
```

## Write to a file

```ruby
fh = File.new("sample.txt", "r+")
if fh
#code
else
# Unable to open file
end
```

## Delete a file

```ruby
File.delete("filename.txt")
```

## Rename a file

```ruby
File.rename("old-filename", "new-filename")
```


