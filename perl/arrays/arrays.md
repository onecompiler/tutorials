Array is a variable which gives dynamic storage for a list. Since list is immutable, you can't change the list items directly. If you want to modify the list, you need to store it in an array variable.

Array variable begins with `@`. 

### Tip
`$` sign looks like S in the word scalar and `@` looks like a in the word array 

## Example

```perl
my @fruits = qw(Apple Orange Grapes Kiwi Watermelon Banana);
```
# How to access array elements

Array elements can be accessed by using indices. Array indices starts from `0`.  Array[n-1] can be used to access nth element of an array.

## Examples

```perl
my @fruits = qw(Apple Orange Grapes Kiwi Watermelon Banana);
print "Array elements are: ";
print("@fruits" ,"\n");

print "First element of the array is: ";
print "$fruits[0]\n";
```

### Check result [here](https://onecompiler.com/perl/3vnrab9dj)

# Array Operations

|Array Operations| Description|
|----|----|
| $count| returns the number of elements in the array|
| $# | returns the highest index of an array|
| push() | appends one or more elements to the end of an array |
|unshift() | adds one or more elements to the front of the array |
| pop() | removes the last element from the end of an array|
| sort() | used to sort an array in alphabetical or numerical order. |

## Examples

```perl
@num = qw( 1 2 3 4 5);

# To get the count of number of elements of an array
$count = scalar @num;
print "Number of elements present in an array: $count \n";

# To find highest index of an array
$last = $#num;
print "Highest index is: $last \n";


# Add elements to an array at the end
push(@num, 6);
push(@num, 7);
push(@num, 8);
print "Array elements after adding : ";
print @num;
print "\n";

# Add element at beginning
unshift(@num, 0);
print "Array elements after using unshift : ";
print @num;
print "\n";

# removing last element from an array
$ele = pop(@num);
print ("Removed element: $ele \n");
```

### Check result [here](https://onecompiler.com/perl/3vnrdbrrk)
