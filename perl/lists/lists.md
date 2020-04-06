List is a series of scalar values separated by commas and enclosed in round brackets. A list is immutable and hence once defined, you cannot change it directly.

## Example

```perl
(); # empty list

(1,2,3,4,5); # integer list

("Hello", "World"); #string list

("happy", 16) # list with different types of data
```
List elements can be displays using print operator like below:

```perl
print(()); # prints nothing as the list is empty
print("\n");
print(1,2,3,4,5); # prints 12345
print("\n");
print("Hello", "World"); # prints HelloWorld
print("\n");
print("happy", 16) # prints happy16
```
### Check result [here](https://onecompiler.com/perl/3vnqnmnss)

Lists are indexed like arrays and indices starts from zero, means `list[0]` refers to first element and to access nth element, you should use `list[n-1]`.

```perl
print((1,2,3,4,5)[0]); # to access first element
print "\n"; 

print((1,2,3,4,5)[2]); # to access third element
print "\n"; 

print((1,2,3,4,5)[4]); # to access last element
print "\n"; 
```
### Check result [here](https://onecompiler.com/perl/3vnr7p2q9)

## qw() function

qw() function allows you to get a list by extracting words out of a string using `space` as a delimiter. Both gives same result.

```perl
print('Apple','Mango','Orange','Kiwi'); 
print("\n");
 
print(qw(Apple Mango Orange Kiwi)); 
print("\n");
```
### Check result [here](https://onecompiler.com/perl/3vnra587p)