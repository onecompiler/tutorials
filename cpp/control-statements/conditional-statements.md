When ever you want to perform a set of operations based on a condition(s) If / If-Else / Nested ifs are used.

You can also use if-else , nested IFs and IF-ELSE-IF ladder when multiple conditions are to be performed on a single variable.

## 1. If

### Syntax

```c
if(conditional-expression)
{
    //code
}
```
### Example

```c
#include <iostream>
using namespace std;

int main() 
{
  int x = 30;
  int y = 30;

  if ( x == y) {
    cout << "x and y are equal";
  }
}
```
#### Check result [here](https://onecompiler.com/cpp/3vmbfhgq9)

## 2. If-else

### Syntax

```c
if(conditional-expression)
{
    //code
} else {
    //code
}
```
### Example

```c
#include <iostream>
using namespace std;

int main() 
{
  
  int x = 30;
  int y = 20;

  if ( x == y) {
    cout << "x and y are equal";
  } else {
    cout <<"x and y are not equal";  
  }
}
```
#### Check result [here](https://onecompiler.com/cpp/3vmbfnjv3)

## 3. If-else-if ladder

### Syntax
```c
if(conditional-expression-1)
{
    //code
} else if(conditional-expression-2) {
    //code
} else if(conditional-expression-3) {
    //code
}
....
else {
    //code
}
```

### Example
```c
#include <iostream>
using namespace std;

int main() 
{
  int age = 15;

    if ( age <= 1 && age >= 0) {
      cout << "Infant";
    } else if (age > 1 && age <= 3) {
        cout << "Toddler";
    } else if (age > 3 && age <= 9) {
        cout << "Child";
    } else if (age > 9 && age <= 18) {
        cout << "Teen";
    } else if (age > 18) {
        cout << "Adult";
    } else {
        cout << "Invalid Age";
    }

}
```
#### Check result [here](https://onecompiler.com/cpp/3vmbfzw6a)

## 4. Nested-If

Nested-Ifs represents if block within another if block. 

### Syntax
```c
if(conditional-expression-1) {    
     //code    
          if(conditional-expression-2) {  
             //code
             if(conditional-expression-3) {
                 //code
             }  
    }    
}
```

### Example
```c
#include <iostream>
using namespace std;

int main() 
{
 int age = 50;
 char resident = 'Y';
 if (age > 18)
  {
    if(resident == 'Y'){
      cout << "Eligible to Vote";
    }
  }
}
```
#### Check result [here](https://onecompiler.com/cpp/3vmbg4fkb)

## 5. Switch

Switch is an alternative to IF-ELSE-IF ladder and to select one among many blocks of code.

### Syntax

```c
switch(conditional-expression){    
case value1:    
 //code    
 break;  //optional  
case value2:    
 //code    
 break;  //optional  
...    
    
default:     
 //code to be executed when all the above cases are not matched;    
} 
```
### Example
```c
#include <iostream>
using namespace std;

int main() 
{
   int day = 3;
      
      switch(day){
        case 1: cout << "Sunday";
        break;
        case 2: cout << "Monday";
        break;
        case 3: cout << "Tuesday";
        break;
        case 4: cout << "Wednesday";
        break;
        case 5: cout << "Thursday";
        break;
        case 6: cout << "Friday";
        break;
        case 7: cout << "Saturday";
        break;
        default: cout << "Invalid day";
        break; 
      }
}
```
####  Check Result [here](https://onecompiler.com/cpp/3vmbg85we)

## 5. Short handed if else / Ternary Operations

There is also an abbreviation for if else called the ternary operator because it consists of three operands. Can be used to replace multiple lines of code with a single line. Often used to replace simple if else statements.
.

### Syntax

```c
variable = (condition) ? expressionTrue : expressionFalse;
```
### Example
Instead of writing
```c
#include <iostream>
using namespace std;

int main() 
{
  int age = 20;
  if (age < 18) {
    cout << "Underaged.;
} else {
    cout << "Eligible to vote";
}
}
```
 We can Simply Write
 ```c
#include <iostream>
using namespace std;

int main() 
{
  int age = 20;
  string result = (age < 18) ? "Underaged" : "Eligible to vote";
  cout << result;
}
```

####  Check Result [here](https://onecompiler.com/cpp/3ykkar3su)

