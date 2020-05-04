Function is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

## How to define a Function

```c
func function_name(parameters) return_type{
  //code
}
```
In the above syntax parameters and return type are optional. Also you can return multiple values using Go Functions.

## How to call a Function

```c
function_name(parameters)
```
# Examples

## 1. Go Functions with return value

```c
package main
import "fmt"

func add() int {  
   a := 10;
   b := 20;
   sum := a + b;
   return sum  
}  
func main() {  
   val := add()  
   fmt.Println(val)  
}  
```
### Check result [here](https://onecompiler.com/go/3vpxet6rb)

## 2. Go function with arguments

```c
package main
import "fmt"

func main() {  
   fmt.Println(sum(10,20,30));  
}  

func sum(args ... int) int {  
  value := 0  
   
  for _,x  := range args{  
    value += x  
  }  
  return value  
}  
```

### Check Result [here] (https://onecompiler.com/go/3vpxf296m)

## 3. Go function with multiple return values

```c
package main
import "fmt"

func main() {  
   fmt.Println(sumAndAverage(10,20,30))  
}  
func sumAndAverage(args ... int)(int,int)  {  
   sumValue:=0  
   avgValue:=0  
   i := 0;
   for _,value  := range args{  
      sumValue += value  
      avgValue -= value 
      i += 1;
   }  
   finalAvgValue := avgValue/i;
   return sumValue,finalAvgValue  
}  
```
### Check Result [here](https://onecompiler.com/go/3vpxfdnub)
