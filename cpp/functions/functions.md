Functions is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Functions allows you to divide your large lines of code into smaller ones. Usually the division happens logically such that each function performs a specific task and is up to developer.

Two types of functions are present in C++:

1. Library Functions

Library functions are the in-built functions which can be used directly by the programmers. No need to define them as they are already defined in header files.

2. User defined functions

User defined functions are the ones which are written by the programmer based on the requirement. Programmers define them on their own.

## How to declare a Function

```c
return_type function_name(parameters);
```

## How to call a Function

```c
function_name (parameters)
```
## How to define a Function
```c
return_type function_name(parameters){  
//code
}
```
# Different ways of calling a user-defined functions

You can call functions in different ways as given below based on the criteria.

1. Function with no arguments and no return value.
2. Function with no arguments and a return value.
3. Function with arguments and no return value.
4. Function with arguments and a return value.

Let's look examples for the above types.

## 1. Function with no arguments and no return value.

```c
#include <iostream>
using namespace std;
void greetings();  
int main()
{
    greetings();
}

void greetings()  
{  
    cout << "Hello world!!";  
}  
```
### check result [here](https://onecompiler.com/cpp/3vmbjbjgn)

## 2. Function with no arguments and a return value.

```c
#include <iostream>
using namespace std;
int sum();
int main()
{
  int result;
  result = sum();
  cout << "Sum : " << result;
}

int sum() {
   int x, y, sum;
   x = 10;
   y = 20;
 
   sum = x + y;
   return (sum); // returning sum value
}
```
### check result [here](https://onecompiler.com/cpp/3vmbjd9yd)

## 3. Function with arguments and no return value.

```c
#include <iostream>
using namespace std;

int sum(int , int );
int main()
{
  int x= 10, y = 20;
  sum(x,y); // passing arguments to function sum
}

int sum(int x , int y) {
   int sum;
   sum = x + y;
   cout << "Sum : " << sum;
   return 0;
}
```

### Check result [here](https://onecompiler.com/cpp/3vmbjgdvn)

## 4. Function with arguments and a return value.

```c
#include <iostream>
using namespace std;

int sum(int , int ); // function declaration

int main()
{
  int x= 10, y = 20;
  int result;
  
  result = sum(x, y); // passing arguments to function sum
  cout << "Sum : " << result;

  
}

int sum(int x , int y) { //function definition
   int sum;
   sum = x + y;
   return sum; // returning sum value
}
```
### Check result [here](https://onecompiler.com/cpp/3vmbhyses)

## calling a function inside same function(recursion):
```c
#include <iostream>
using namespace std;

int increase(int ); // function declaration

int main()
{
  int x= 10;
  int result;
  
  result = increase(x); // passing arguments to function increase
  cout << "Result after increasing : " << result;

  
}

int increase(int x ) { //function definition
   int new_result = x + 1;
   return increase(new_result); // calling itself 
}
```
### Check result [here](https://onecompiler.com/cpp/3yk82ucqf)
However this will continue increasing the value of the parameter due to which it will call the same function again and again, leading to infinite recursion and eventually a stack overflow. To avoid this we can set a limit which will stop the function from calling itself after we cross the limit. This is also known as a base case.


```c
#include <iostream>
using namespace std;

int increase(int ); // function declaration

int main()
{
  int x= 10;
  int result;
  
  result = increase(x); // passing arguments to function increase
  cout << "Result after increasing : " << result;

  
}

int increase(int x ) { //function definition
   if(x > 20) return x; //base case
   int new_result = x + 1;
   return increase(new_result); // calling itself 
}
```
### Check result [here](https://onecompiler.com/cpp/3yk82ucqf)

## Lambda Functions (C++11 and later)

Lambda functions are anonymous functions that can be defined inline. They are particularly useful for short functions that are used only once or in specific contexts like with STL algorithms.

### Lambda Syntax
```cpp
[capture](parameters) -> return_type { body }
```

### Basic Lambda Examples

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // Simple lambda with no parameters
    auto greet = []() {
        cout << "Hello from lambda!" << endl;
    };
    greet();
    
    // Lambda with parameters
    auto add = [](int a, int b) {
        return a + b;
    };
    cout << "Sum: " << add(5, 3) << endl;
    
    // Lambda with capture
    int multiplier = 10;
    auto multiply = [multiplier](int x) {
        return x * multiplier;
    };
    cout << "Result: " << multiply(5) << endl;
    
    // Lambda with STL algorithms
    vector<int> numbers = {1, 2, 3, 4, 5};
    for_each(numbers.begin(), numbers.end(), [](int n) {
        cout << n * 2 << " ";
    });
    cout << endl;
    
    return 0;
}
```

### Check result [here](https://onecompiler.com/cpp/lambda-example)
