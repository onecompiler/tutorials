Pointer is a variable which holds the memory information(address) of another variable of same data type.

* Pointers helps in dynamic memory management.
* Improves execution time.
* More efficient while handling arrays and structures.
* You can pass function as an argument to another function only with the help of pointers.
<hr>

## Syntax

Pointer's data type should match with the data type of the variable which is getting pointed.

```c
datatype* pointername;
// or
datatype *pointername;
```
<hr>

## Example: 1

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
<br>

## Example: 2

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
<br>

Pointer has the ability to store address of more than one elements or a cells of elements. 

## Example: 3

```c
#include <iostream>
using namespace std;

int main()
{
    int arr[3] = { 2, 3, 4 }; // arr is an array of size 3
    int* ptr = arr; // syntax for making a pointer point to an array
    cout << ptr << endl; //prints the address of arr[0] beacuse ptr stores the address of the first element of arr
    cout << *ptr << endl; //prints the value of the first element i.e, 2
    
    //Now let us see how can we print the whole array by using the pointer
    for (int i = 0; i < 3; i++){
    
        cout << *(ptr + i) << endl;
        
    }
    
    return 0;
}
```
### Check result [here](https://onecompiler.com/cpp/3yjazmygf)
<hr>

## Pointer Arithmetic

- Post or Pre increment(++)
- Post or Pre decrement(--)
- Addition(+)
- Subtraction(-)

### Example

```c
#include <iostream>
using namespace std;

void operations()
{
	//Declare an array
	int v[3] = {10, 100, 200};
	
	//Declare pointer variable
	int *ptr;
	
	//Assign the address of v[0] to ptr
	ptr = v;
	
	for (int i = 0; i < 3; i++)
	{
		cout << "Value at ptr: " << ptr << "\n";
		cout << "Value at *ptr: " << *ptr << "\n";
		
		// Increment pointer ptr by 1 i.e. point to the next address
		ptr++;
	}
}

int main()
{
	operations();
    return 0;
}
```
### Check result [here](https://onecompiler.com/cpp/3ykbjd94f)