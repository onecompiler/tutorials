As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

There are three different types of Data types in C++.

| Types | Data-type|
|----|----|
|Basic | int, char, float, double, short, short int, long int etc |
|Derived | array, pointer etc |
|User Defined Data Type | structure, enum, Class, Union, Typedef |


## 1. Basic data types

Basic data types are generally arithmetic types which are based on integer and float data types. They support both signed and unsigned values. Below are some of the oftenly used data types

| Data type | Description | Range | Memory Size|
|----|----|----|----|----|
| int| used to store whole numbers|-32,768 to 32,767|2 bytes| 
|short int| used to store whole numbers|-32,768 to 32,767| 2 bytes|
|long int| used to store whole numbers|	-2,147,483,648 to 2,147,483,647| 4 bytes|
|float| used to store fractional numbers|6 to 7 decimal digits| 4 bytes|
|double| used to store fractional numbers|15 decimal digits| 8 bytes|
|char|used to store a single character|one character|1 bytes|
|bool| Boolean data type| 1 byte|

### Examples

```c
#include <iostream>
using namespace std;

int main() 
{

int x = 90;
int y = sizeof(x);
cout << "size of x is: " << y << endl;

float f = 3.14;
cout << "size of float is: " << sizeof(f) << endl;

double d = 2.25507e-308;
cout << "size of double is: " << sizeof(d) << endl;

char c = 'a';
cout << "size of char is: " << sizeof(c) << endl;

return 0;
}

```
#### Run [here](https://onecompiler.com/cpp/3vm83rs6y)

## Modern C++ Type Features (C++11 and later)

### Auto Keyword for Type Inference

The `auto` keyword allows the compiler to automatically deduce the type of a variable from its initializer. This makes code more concise and easier to maintain.

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main() {
    // Basic auto usage
    auto x = 42;           // x is int
    auto y = 3.14;         // y is double
    auto z = 'a';          // z is char
    auto name = "Hello";   // name is const char*
    
    // With STL containers
    auto numbers = vector<int>{1, 2, 3, 4, 5};
    auto text = string("Modern C++");
    
    // Auto with function return types
    auto result = sqrt(16.0);  // result is double
    
    cout << "x: " << x << " (type: int)" << endl;
    cout << "y: " << y << " (type: double)" << endl;
    cout << "z: " << z << " (type: char)" << endl;
    cout << "result: " << result << " (type: double)" << endl;
    
    return 0;
}
```

### Fixed-Width Integer Types (C++11)

C++11 introduced fixed-width integer types that guarantee specific sizes across different platforms.

```cpp
#include <iostream>
#include <cstdint>
using namespace std;

int main() {
    // Fixed-width signed integers
    int8_t   small_int = 127;      // 8-bit signed (-128 to 127)
    int16_t  medium_int = 32767;   // 16-bit signed (-32,768 to 32,767)
    int32_t  normal_int = 2147483647; // 32-bit signed
    int64_t  large_int = 9223372036854775807LL; // 64-bit signed
    
    // Fixed-width unsigned integers
    uint8_t  small_uint = 255;     // 8-bit unsigned (0 to 255)
    uint16_t medium_uint = 65535;  // 16-bit unsigned (0 to 65,535)
    uint32_t normal_uint = 4294967295U; // 32-bit unsigned
    uint64_t large_uint = 18446744073709551615ULL; // 64-bit unsigned
    
    // Using auto with fixed-width types
    auto fast_int = int_fast32_t{1000};  // At least 32 bits, optimized for speed
    auto least_int = int_least16_t{500}; // At least 16 bits, optimized for size
    
    cout << "int8_t: " << static_cast<int>(small_int) << " (size: " << sizeof(small_int) << " bytes)" << endl;
    cout << "int32_t: " << normal_int << " (size: " << sizeof(normal_int) << " bytes)" << endl;
    cout << "uint64_t: " << large_uint << " (size: " << sizeof(large_uint) << " bytes)" << endl;
    
    return 0;
}
```

### Modern Examples with Auto

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <string>
using namespace std;

int main() {
    // Auto with complex types
    auto grades = map<string, int>{
        {"Alice", 95},
        {"Bob", 87},
        {"Charlie", 92}
    };
    
    // Auto in range-based for loops
    for (const auto& student : grades) {
        cout << student.first << ": " << student.second << endl;
    }
    
    // Auto with lambda functions
    auto multiply = [](auto a, auto b) {
        return a * b;
    };
    
    cout << "5 * 3 = " << multiply(5, 3) << endl;
    cout << "2.5 * 4.0 = " << multiply(2.5, 4.0) << endl;
    
    // Auto with iterators
    auto numbers = vector<int>{10, 20, 30, 40, 50};
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 2. Derived or User defined Data types 

Derived Data types and user defined dta types are the ones which are derived from fundamental data types and are defined by user. Arrays, Pointers, functions etc. are examples of derived data types. Enum, Structures, Class, union, enum, typedef etc are User defined data types. Let's learn more about them in next chapters.
