# **Dynamic Memory Allocation**

<br>

### **What is the difference between Static and Dynamic Memory Allocation ?**
<br>

There are two types of memory in our machine, one is _Static Memory_ and another one is _Dynamic Memory_ ; both the memory are managed by our Operating System. Our operating system helps us in the allocation and deallocation of memory blocks either during compile-time or during the run-time of our program. <br>
When the memory is allocated during compile-time it is stored in the Static Memory and it is known as **Static Memory Allocation**, and when the memory is allocated during run-time it is stored in the Dynamic Memory and it is known as **Dynamic Memory Allocation.**

<br>
Now, let us see some differences in Static Memory Allocation and Dynamic Memory Allocation.

<br>
<br>


| Static Memory Allocation  | Dynamic Memory Allocation | 
|-------------------------- | ------------------------- |
| Constant (Invariable) memory is reserved at compile-time of our program that can't be modified. |  Dynamic (Variable) memory is reserved at run-time of our program that can be modified. |  |
| It is used at **compile-time** of our program and is also known as **compile-time memory allocation.**|It is used at **run-time** of our program and is also known as **run-time memory allocation.**||
|We **can't** allocate or deallocate a memory block during run-time. |We **can** allocate and deallocate a memory block during run-time. ||
|**Stack** space is used in Static Memory Allocation.|**Heap** space is used in Dynamic Memory Allocation.||
|It doesn't provide reusability of memory, while the program is running. So, it is less efficient.|It provides reusability of memory, while the program is running. So, it is more efficient.||

<br>
<br>

## Introduction to Dynamic Memory Allocation in C

<br>

**Dynamic Memory Allocation** is a process in which we allocate or deallocate a block of memory during the run-time of a program. 
<br>
It can also be referred to as a procedure to use Heap Memory in which we can vary the size of a variable or Data Structure (such as an Array) during the lifetime of a program using the library functions.<br>
C provides some functions to achieve these tasks.<br> There are 4 library functions provided by C defined under **<stdlib.h>** header file to facilitate dynamic memory allocation in C programming. They are: 

1. malloc()<br>
2. calloc()<br>
3. free()<br>
4. realloc()<br>

Now, let's discuss these functions in detail -
<br>
<br>
## 1. malloc()
<br>
The name "malloc" stands for memory allocation. <br>
malloc() is used to dynamically allocate a single large block of memory with the specified size.<br>
<br>

**General Syntax-**

>(cast-data-type *)malloc(size-in-bytes); 

<br>
-->  malloc() function takes size in bytes as an argument and returns a void pointer, so we have to type cast the malloc() function to the required data type.
<br>-->  malloc() does not initialises the allocated memory block, so initially it contains a garbage value.
<br>
<br>

**For Example :**
>ptr = (int*) malloc(200 * sizeof(int));

<br> Since the size of int is 4 bytes, this statement will allocate 800 bytes of memory. And, the pointer ptr holds the address of the first byte in the allocated memory.
<br>
The expression results in a _NULL_ pointer if the memory cannot be allocated.
<br>
<br>

### C Program : 

>// C Program to dynamically allocate an int ptr using malloc() <br><br>
#include <stdio.h>
<br>
#include <stdlib.h>
<br><br>
int main() {
    <br><br>
	// Dynamically allocated variable, sizeof(char) = 1 byte. <br><br>
	char *ptr = (char *)malloc(sizeof(char));
    <br>
	if (ptr == NULL) { <br>
		printf("Memory Error!\n");<br>
	} else {<br>
		*ptr = 'S';<br>
		printf("%c", *ptr);<br>
	}<br><br>
    return 0;<br>
}<br> <br>

### OUTPUT :
>S

<br><br>


## 2. calloc()
<br>
The name "calloc" stands for contiguous allocation.<br>
 calloc() is used to dynamically allocate the specified number of blocks of memory of the specified type.<br>It is generally used to allocate a sequence of memory blocks (contiguous memory) like an array of elements.<br> It is very much similar to malloc() but has two different points and these are:<br>
--> It initializes each block with a default value ‘0’.<br>
--> It has two parameters or arguments as compare to malloc().<br>
<br>

**General Syntax-**

>(cast-data-type *)calloc(n , size-in-bytes);
 

<br>
--> calloc() function takes two arguments, first is the size of the array (number of elements i.e. n here) and second is the sizeof() data type (in bytes) of which we have to make an array.
<br>
<br>

**For Example :**
>ptr = (int*) calloc(30, sizeof(int));


<br>This statement allocates contiguous space in memory for 30 elements each with the size of the int.
<br>
The expression results in a _NULL_ pointer if the memory cannot be allocated.
<br>
<br>

### C Program : 
<br>

>// C Program to dynamically allocate an int ptr using calloc()<br><br>#include<stdio.h>  
#include<stdlib.h>  
int main(){  
 int n,i,*ptr,sum=0;    
    printf("Enter number of elements: ");    
    scanf("%d",&n);    
    ptr=(int*)calloc(n,sizeof(int));  //memory allocated using calloc    
    if(ptr==NULL)                         
    {    
        printf("Sorry! unable to allocate memory");    
        exit(0);    
    }    
    printf("Enter elements of array: \n ");    
    for(i=0;i<n;++i)    
    {    
        scanf("%d",ptr+i);    
        sum+=*(ptr+i);    
    }    
    printf("Sum=%d",sum);    
    free(ptr);    
return 0;  
}    
<br> <br>

### OUTPUT :
>Enter number of elements : 3 <br>
Enter elements of array:<br>
10 <br>
10 <br>
10 <br>
Sum=30

<br><br>


## 3. realloc()
<br>
The name “realloc” stands for “re-allocation”.
<br> It is used to dynamically change the size of a previously allocated memory.<br> In other words, if the memory previously allocated with the help of malloc() or calloc() is insufficient, realloc() can be used to dynamically re-allocate memory. <br>Re-allocation of memory maintains the already present value and new blocks will be initialized with the default garbage value.
<br>
<br>

**General Syntax-**
>realloc(ptr, new-size-in-bytes)

<br>
Here, <br>
ptr : pointer to the memory area to be reallocated<br>
new_size : new size of the array in bytes <br>
realloc() takes two arguments, one is the pointer pointing the the memory which is to be realloacted and second is the new size in bytes.
<br>
<br>

**For Example :**
> ptr = realloc(ptr, n2 * sizeof(int));

<br>
In the above statement, n2 is the number of blocks to be reallocated where size of each block equals to the size of int.

<br>
<br>

## 4. free()
<br>
free() method, as the name suggests, is used to dynamically de-allocate the memory. <br>
The memory allocated using functions malloc() and calloc() is not de-allocated on their own. Hence the free() method is used, whenever the dynamic memory allocation takes place. <br>
It helps to reduce wastage of memory by freeing it.
<br>
<br>

**General Syntax-**
>free( pointer );

<br>
free() takes one argument and it is the pointer containing the address of the memory block that is to be freed or deallocated.

<br><br>
**For Example :**
>free(arr);

<br>
In the above statement, we are deallocating memory block(s) pointed by a pointer arr in the memory.
<br>
After deallocation of memory, arr acts as a Dangling Pointer and is not pointing to any location. It now contains some garbage value.
<br><br>

**NOTE** : We can deallocate a memory block without using free() function, alternatively using the realloc() function.
<br>
   realloc(ptr, 0);  statement is the same as free(ptr); .
 


















