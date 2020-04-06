Perl is a loosely typed language and hence you no need to define the type of the data when you define variables. Perl interpreter has the capability to choose the type based on the context of the data itself.

Perl has three basic data types: 

1. Scalars
2. Arrays 
3. Hashes 

## 1. Scalars

Scalars are simple variables like integers, strings, or references(memory address of a variable) etc. They are preceded by `$`

### Example

```perl
$x = 10;
$y = 20;
$sum = $x + $y;
print($sum);
```
### Check Result [here](https://onecompiler.com/perl/3vnhpvaqe)

## 2. Arrays

Array is an ordered list of scalar variables. They are preceded by `@`. Array elements can be accessed by using indices and starts with 0.

### Example

```perl
@arr = (1,2,3,4,5);

print "arr0: $arr[0]\n";
print "arr1: $arr[1]\n";
print "arr2: $arr[2]\n";
print "arr3: $arr[3]\n";
print "arr4: $arr[4]\n";
```
### Check result [here](https://onecompiler.com/perl/3vnjt3wtv)

## 3. Hashes

Hash is an unordered set of key/value pairs. They are preceded by `%`. Hashes can be accessed by using subscripts.

### Example

```perl
%hash1 = ('Name' , 'OneCompiler', 'Category' , 'Learning');
print "hash1{'Name'} = $hash1{'Name'}\n";
print "hash1{'Category'} = $hash1{'Category'}\n";
```
Below is another way of declaring hash values.
```perl
%hash2 = ('Name' => 'OneCompiler', 'Category' => 'Learning');
print "hash2{'Name'} = $hash2{'Name'}\n";
print "hash2{'Category'} = $hash2{'Category'}\n";
```
### Check Result [here](https://onecompiler.com/perl/3vnjugavk)

