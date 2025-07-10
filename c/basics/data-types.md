As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

There are four different types of Data types in C.

| Types | Data-type|
|----|----|
|Basic | int, char, float, double|
|Derived | array, pointer, structure, union|
|Enumeration | enum|
|Void |	void|


## 1. Basic data types

Basic data types are generally arithmetic types which are based on integer and float data types. They support both signed and unsigned values. Below are some of the oftenly used data types

| Data type | Description | Range | Memory Size| Format specifier|
|----|----|----|----|----|
| int| used to store whole numbers|-2,147,483,648 to 2,147,483,647|4 bytes| %d|
|short int| used to store whole numbers|-32,768 to 32,767| 2 bytes|%hd|
|long int| used to store whole numbers|	-2,147,483,648 to 2,147,483,647| 4 bytes|%li|
|float| used to store fractional numbers|6 to 7 decimal digits| 4 bytes|%f|
|double| used to store fractional numbers|15 decimal digits| 8 bytes|%lf|
|char|used to store a single character|-128 to 127 (signed) or 0 to 255 (unsigned)|1 byte|%c|
|unsigned char|used to store a single character|0 to 255|1 byte|%c|
|unsigned int| used to store positive whole numbers|0 to 4,294,967,295| 4 bytes|%u|
|unsigned long| used to store positive whole numbers|0 to 4,294,967,295 (minimum)| 4 or 8 bytes|%lu|
|long long int| used to store very large whole numbers|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807| 8 bytes|%lld|
|unsigned long long| used to store very large positive numbers|0 to 18,446,744,073,709,551,615| 8 bytes|%llu|
|_Bool or bool|used to store boolean values|0 or 1|1 byte|%d|

### Additional Data Types

#### size_t and ptrdiff_t
- **size_t**: An unsigned integer type used to represent the size of objects in bytes and is guaranteed to be able to store the maximum size of any object. Format specifier: %zu
- **ptrdiff_t**: A signed integer type used to represent the difference between two pointers. Format specifier: %td

#### Fixed-width Integer Types (from stdint.h)
C99 introduced fixed-width integer types that guarantee exact sizes across different platforms:

| Data type | Description | Size | Format specifier|
|----|----|----|----|
| int8_t | Signed 8-bit integer | 1 byte | %d (after casting) or use PRId8 |
| uint8_t | Unsigned 8-bit integer | 1 byte | %u (after casting) or use PRIu8 |
| int16_t | Signed 16-bit integer | 2 bytes | %d or use PRId16 |
| uint16_t | Unsigned 16-bit integer | 2 bytes | %u or use PRIu16 |
| int32_t | Signed 32-bit integer | 4 bytes | %d or use PRId32 |
| uint32_t | Unsigned 32-bit integer | 4 bytes | %u or use PRIu32 |
| int64_t | Signed 64-bit integer | 8 bytes | %lld or use PRId64 |
| uint64_t | Unsigned 64-bit integer | 8 bytes | %llu or use PRIu64 |

### Examples

```c
#include <stdio.h>
#include <float.h>
#include <stdbool.h>
#include <stdint.h>
#include <inttypes.h>

int main()
{
    // Basic types with proper sizeof usage (note: sizeof returns size_t)
    int x = 90;
    printf("size of int: %zu bytes\n", sizeof(int));
    printf("value of x: %d\n", x);
    
    // Unsigned types
    unsigned int ui = 4294967295U;
    printf("size of unsigned int: %zu bytes\n", sizeof(unsigned int));
    printf("max unsigned int: %u\n", ui);
    
    // Long long
    long long ll = 9223372036854775807LL;
    printf("size of long long: %zu bytes\n", sizeof(long long));
    printf("max long long: %lld\n", ll);
    
    // Float and double
    float f = 3.14f;
    printf("size of float: %zu bytes\n", sizeof(float));
    
    double d = 2.25507e-308;
    printf("size of double: %zu bytes\n", sizeof(double));
    
    // Character types
    char c = 'a';
    unsigned char uc = 255;
    printf("size of char: %zu byte\n", sizeof(char));
    printf("char value: %c, unsigned char value: %u\n", c, uc);
    
    // Boolean type
    bool flag = true;
    printf("size of bool: %zu byte\n", sizeof(bool));
    printf("bool value: %d\n", flag);
    
    // Fixed-width types
    int32_t fixed32 = 2147483647;
    uint64_t fixed64u = 18446744073709551615ULL;
    printf("int32_t value: %" PRId32 "\n", fixed32);
    printf("uint64_t value: %" PRIu64 "\n", fixed64u);
    
    // size_t and ptrdiff_t
    size_t size = sizeof(int);
    printf("size_t example: %zu\n", size);
    
    return 0;
}
```
### Check result [here](https://onecompiler.com/c/3vkf2hsrg)

## 2. Derived Data types

Derived Data types are the ones which are derived from fundamental data types. Arrays, Pointers, Structures, etc. are examples of derived data types. Let's learn more about them in next chapters.

## 3. Enumeration Data types

Enumeration Data type is a user-defined data type in C. `enum` keyword is used to declare a new enumeration types in C. 

### Syntax

```c
enum name{constant1, constant2, constant3, ....... };
```
### Example

```c
#include<stdio.h> 
  
enum month{January, February, March, April, May, June, July, August, September, October, November, December};
  
int main() 
{ 
    enum month name; 
    name = June; 
    printf("%d",name); 
    return 0; 
} 
```
### Check result [here](https://onecompiler.com/c/3vkf3vuuu)

## 4. Void Data types

Void specifies that there is no return value. Generally void is used in the below situations.

* If the funtion has return type mentioned as Void, then it specifies that the function returns no value.
* A function with out any parameters can accept void. For example., char greetings(void)
* A pointer with type specified as void represents the address of an object but not it's type. Let's learn more about pointers in next chapters.

