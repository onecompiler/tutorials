Pointer is a variable which holds the memory information(address) of another variable of same data type.

* Pointers helps in dynamic memory management.
* Improves execution time.
* More efficient while handling arrays and structures.
* You can pass function as an argument to another function only with the help of pointers.

## Syntax

pointer data type should match with the data type of the variable which is getting pointed.

```c
datatype *pointername;
```

## Example

```c
#include <stdio.h>

int main()
{
    int num = 10;     
    int *ptr;   // pointer variable

    ptr = &num;

   printf("Value of the variable: %u\n", *ptr);

   printf("Value at the address: %d\n", ptr);

   return 0;
}

```
## Practice with more examples [here](https://onecompiler.com/c)