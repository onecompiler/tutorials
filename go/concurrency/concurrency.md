Go supports concurrent execution of tasks using Goroutines and Channels.

# Goroutines

Usually when a function is called, the control goes to that function and returns back after it's execution is completed and then the remaining code will get executed. In this process wait time will be there and it depends on the function execution.

But the beauty of Goroutines is, it will not wait for the execution of function to complete. It will just call the function and continue to execute the rest of the code. Function execution will happen in concurrent with the rest of code execution.

`go` keyword is used to invoke Goroutines.

## Syntax

```c
go function-name([arguments])
```

## Example

```c
go sum()
```
In the below example, we can see the execution of main function gets finished even before goroutine function starts it's execution.

```c
package main
import "fmt"
    
func function() {
	for i:=1; i<=5; i++ {
		fmt.Println("Control is in function")
	}
}

func main() {
	go function() //invoking goroutine
	
	for i:=1; i<=5; i++ {
		fmt.Println("Control is in main")
	}
}
```
### Check Result [here](https://onecompiler.com/go/3vq829rj5)


To understand better, lets use Sleep function in main to see the execution of goroutine function.

```c
package main
import "fmt"
import "time"
    
func function() {
	for i:=1; i<=5; i++ {
		fmt.Println("Control is in function")
	}
}

func main() {
	go function() //invoking goroutine
	
	for i:=1; i<=5; i++ {
		if i==4 {
		  time.Sleep(10 * time.Second)
		}
		fmt.Println("Control is in main")
	}
}
```
### Check Result [here](https://onecompiler.com/go/3vq7zmemd)

# Channels

Channels are like medium where we send values from one goroutine to another. Only one Goroutine has access to a data item at any given time and thus it maintains synchronization.

A channel is bidirectional means same channel can be used for both sending and receiving data.

## How to declare channels

```c
var channel-name chan Data-Type
```
or

```c
channel-name:= make(chan Data-Type)
```

## Example

```c
package main 
  
import "fmt"
  
func greetings(ch chan string) { 
  
    fmt.Println("Hello ", <-ch) 
} 

func main() { 
    
    // Creating a channel 
    ch := make(chan string) 
  
    go greetings(ch) 
    ch <- "foo" 
     
} 
```

### Check Result [here](https://onecompiler.com/go/3vq8384ps)
