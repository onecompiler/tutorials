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
package main
import "fmt"

func main() {
  x := 30;
  y := 30;

  if ( x == y) {
    fmt.Printf("x and y are equal");
  }

}
```
### Check result [here](https://onecompiler.com/go/3vpq3azq3)

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
package main
import "fmt"

func main() {
  x := 30;
  y := 20;

  if ( x == y) {
    fmt.Printf("x and y are equal");
  } else {
      fmt.Printf("x and y are not equal");  
  }

}
```
### Check result [here](https://onecompiler.com/go/3vpq3ewjt)

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
package main
import "fmt"

func main() {
age := 15;

if ( age <= 1 && age >= 0) {
      fmt.Println("Infant");
   } else if (age > 1 && age <= 3) {
        fmt.Println("Toddler");
   } else if (age > 3 && age <= 9) {
        fmt.Println("Child");
   } else if (age > 9 && age <= 18) {
        fmt.Println("Teen");
   } else if (age > 18) {
        fmt.Println("Adult");
   } else {
        fmt.Println("Invalid Age");
   }
}
```
### Check result [here](https://onecompiler.com/go/3vpq3mfy3)

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
package main
import "fmt"

func main() {

age := 50;
resident := 'Y';
if (age > 18) {
  if(resident == 'Y') {
    fmt.Printf("Eligible to Vote");
  }
}
}
```
### Check result [here](https://onecompiler.com/go/3vpq3q7g5)

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
package main
import "fmt"

func main() {
day := 3;
      
switch(day){
  case 1: fmt.Println("Sunday");
  break;
  case 2: fmt.Println("Monday");
  break;
  case 3: fmt.Println("Tuesday");
  break;
  case 4: fmt.Println("Wednesday");
  break;
  case 5: fmt.Println("Thursday");
  break;
  case 6: fmt.Println("Friday");
  break;
  case 7: fmt.Println("Saturday");
  break;
  default:fmt.Println("Invalid day");
  break; 
}

}

```
###  Check Result [here](https://onecompiler.com/go/3vpq434cg)
