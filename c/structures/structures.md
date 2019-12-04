Structures is a user-defined data type where it allows you to combine data of different data types. In a way, Structres are similar to arrays but the difference is in type of the data. Arrays is a collection of similar data but structures combine different types of data.

## How to define a structure

`struct` keyword is used to define a structure. 

```c
struct structure_name {

   member definition;
   member definition;
   ...
   member definition;
} [one or more structure variables]; 
```

## How to declare structure variables

Structure variables can be declared in two ways.

1. Declaring variables seperately from its definition.

```c
struct structure_name variable name;
```

2. Declaring variables along with definition(this method is not recommended)


```c
struct structure_name {

   member definition;
   member definition;
   ...
   member definition;
} one or more structure variables; 
```

## How to access structure members

You can access the structure member using variable_name.membername


## Example

```c
// structure definition
struct mobile {
    char model[30];
    char brand[30];
    int cost; 
}

// Declaring variables seperately from its definition

struct mobile m1, m2;

//Declaring variables along with definition

struct mobile {
    char model[30];
    char brand[30];
    int cost; 
}m1, m2;

//accessing structure variable - brand
```

```c
#include<stdio.h>
#include<string.h>

struct mobile {
    char model[30];
    char brand[30];
    int cost; 
};

int main()
{
   struct mobile m1;

// string copy is used to store value to the structure member where m1 is variable name and brand is structure member name   
    strcpy(m1.brand, "Apple");

// display the value of structure variable 
    printf("Brand of the mobile: %s\n", m1.brand);
    
    return 0;
}
```

## Practice with more examples [here](https://onecompiler.com/c)