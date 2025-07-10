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

## Example : 1

```c
#include <stdio.h>

int main()
{
    int num = 10;     
    int *ptr;   // pointer variable

    ptr = &num;

   printf("Value of the variable: %d\n", *ptr);

   printf("Address stored in pointer: %p\n", (void*)ptr);

   return 0;
}

```
### Check result [here](https://onecompiler.com/c/3vm525v98)


## Example : 2

```c
#include <stdio.h>
int main()
{
  int x = 10, *ptr;

/*ptr = x; // Error because ptr is adress and x is value

*ptr = &x;  // Error because x is adress and ptr is value */

ptr = &x; // valid because &x and ptr are addresses

*ptr = x; // valid because both x and *ptr values 

 printf("Value of *ptr: %d\n", *ptr);
 
 printf("Address of x: %p\n", (void*)&x);
 
 printf("Address stored in ptr: %p\n", (void*)ptr);
 
 printf("Value of x: %d\n", x);

}
```
### Check result [here](https://onecompiler.com/c/3vm52fjwd)

## Null Pointer Checking

Always check if a pointer is NULL before dereferencing it to avoid segmentation faults:

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int *ptr = NULL;  // Initialize to NULL
    
    // Always check before dereferencing
    if (ptr != NULL) {
        printf("Value: %d\n", *ptr);
    } else {
        printf("Pointer is NULL, cannot dereference\n");
    }
    
    // Allocate memory
    ptr = (int*)malloc(sizeof(int));
    if (ptr == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }
    
    *ptr = 42;
    printf("Value: %d\n", *ptr);
    
    free(ptr);
    ptr = NULL;  // Good practice: set to NULL after freeing
    
    return 0;
}
```

## Pointer Arithmetic

Pointers can be incremented, decremented, and used in arithmetic operations:

```c
#include <stdio.h>

int main()
{
    int arr[5] = {10, 20, 30, 40, 50};
    int *ptr = arr;  // Points to first element
    
    printf("Array elements using pointer arithmetic:\n");
    for (int i = 0; i < 5; i++) {
        printf("arr[%d] = %d, address: %p\n", i, *(ptr + i), (void*)(ptr + i));
    }
    
    // Pointer increment
    ptr++;  // Now points to second element
    printf("\nAfter ptr++: value = %d, address: %p\n", *ptr, (void*)ptr);
    
    // Pointer difference
    int *ptr2 = &arr[4];
    printf("Difference between ptr2 and ptr: %ld elements\n", ptr2 - ptr);
    
    return 0;
}
```

## Void Pointers

Void pointers can point to any data type but must be cast before dereferencing:

```c
#include <stdio.h>

int main()
{
    int num = 10;
    float f = 3.14f;
    char c = 'A';
    
    void *void_ptr;
    
    // Void pointer can point to any type
    void_ptr = &num;
    printf("Integer value: %d\n", *(int*)void_ptr);
    
    void_ptr = &f;
    printf("Float value: %.2f\n", *(float*)void_ptr);
    
    void_ptr = &c;
    printf("Character value: %c\n", *(char*)void_ptr);
    
    return 0;
}
```

## Function Pointers

Function pointers allow you to store and call functions dynamically:

```c
#include <stdio.h>

// Function prototypes
int add(int a, int b);
int multiply(int a, int b);
void processArray(int arr[], int size, int (*operation)(int, int));

int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}

void processArray(int arr[], int size, int (*operation)(int, int)) {
    printf("Processing array with function pointer:\n");
    for (int i = 0; i < size - 1; i++) {
        int result = operation(arr[i], arr[i + 1]);
        printf("%d op %d = %d\n", arr[i], arr[i + 1], result);
    }
}

int main()
{
    // Declare function pointer
    int (*func_ptr)(int, int);
    
    // Assign function to pointer
    func_ptr = add;
    printf("Using function pointer for addition: %d\n", func_ptr(5, 3));
    
    // Change function
    func_ptr = multiply;
    printf("Using function pointer for multiplication: %d\n", func_ptr(5, 3));
    
    // Array of function pointers
    int (*operations[])(int, int) = {add, multiply};
    printf("Add: %d, Multiply: %d\n", operations[0](4, 2), operations[1](4, 2));
    
    // Pass function pointer to another function
    int arr[] = {1, 2, 3, 4, 5};
    processArray(arr, 5, add);
    
    return 0;
}
```

