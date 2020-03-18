Pointer is a variable which holds the memory information(address) of another variable of same data type.

* Pointers helps in dynamic memory management.
* Improves execution time.
* More efficient while handling arrays and structures.
* You can pass function as an argument to another function only with the help of pointers.

## Syntax

pointer data type should match with the data type of the variable which is getting pointed.

```c
datatype* pointername;
// or
datatype *pointername;
```

## Example

```c
#include <iostream>
using namespace std;

int main()
{
    int num = 10;     
    int *ptr;   // pointer variable

    ptr = &num;

   cout << "Value of the variable: " << *ptr;

   cout << "\nValue at the address: " << ptr;

   return 0;
}

```
###  Check result [here](https://onecompiler.com/cpp/3vmdfajwb)


## Example : 2

```c
#include <iostream>
using namespace std;

int main() 
{
    int x = 10, *ptr;

/*ptr = x; // Error because ptr is adress and x is value

*ptr = &x;  // Error because x is adress and ptr is value */

ptr = &x; // valid because &x and ptr are addresses

*ptr = x; // valid because both x and *ptr values 

cout << "Value of *ptr: " << *ptr << endl;
 
cout << "Value of &x: " << &x << endl;
 
cout << "Value of ptr: " << ptr << endl;
 
cout << "Value of x: " << x << endl;
}
```
### Check result [here](https://onecompiler.com/cpp/3vmdff4eq)




