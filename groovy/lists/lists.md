List is like stack which is used to store  collection of data items. A List literal is presented as a series of data objects separated by commas and enclosed in square brackets. Data objects present in the list need not to be of same type.

Groovy Lists are indexed like arrays and it starts from zero, means `list[0]` refers to first element.

## Example

```java
def list1 = [ ] // empty list

def list2 = [ 1,2,3,4,5] // integer list

def list3 = ["Hello", "World"] //string list

def list4 = ["happy", 16] // list with different data
```
## Methods

|List methods | Description|
|----|----|
| add() | to append the new value to the end of this List.|
| contains() | Returns true if the list contains the given value.|
| get() | to get the element at the specified position from the List.|
| isEmpty() | Returns true if the given List is empty|
| minus() | to create a new List with the elements of the original list by removing those specified in the collection|
| plus() | to create a new List with the elements of the original list by adding those specified in the collection|
| pop() | to remove the last item from the List |
| remove() | to removes an element at the specified position in the List.|
| reverse() | to create a new List which is the reverse of the elements of the given List|
| size() | to get number of elements present in the List.|
| sort() | Returns a sorted copy of the given List.|

### Example

```java
def msg = ["Hello", "world!", "Happy"]
println msg
msg.add("Learning!!")
println("is message empty? "+msg.isEmpty())
println("Message contains Happy? "+msg.contains("Happy"))
println("Message contains happy? "+msg.contains("happy"))
println("Size of the message: " + msg.size())
println(msg.get(1))
println(msg.pop())
```
### Check result [here](https://onecompiler.com/groovy/3vmt4tz9j)

