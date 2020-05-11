## 1. For

For loop is used to iterate a set of statements based on a condition for a spefic number of times.

### Syntax

```py
for (value in vector) {
  # code
}
```
### Example

```py
day <- c("Monday", "Tuesday","Wednesday", "Thursday","Friday", "Saturday", "Sunday")
for ( i in day) {
   print(i)
}
```

#### Check Result [here](https://onecompiler.com/r/3vs9txvd5)

## 2. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```py
while(condition){  
#code
}  
```
### Example

```py
# printing 1 to 10 using while loop in R
i <- 1
while ( i <= 10) {
   print(i)
   i = i+1;
}
```
#### Check result [here](https://onecompiler.com/r/3vs9u7mhw)

## 3. Repeat

Repeat executes a block of code again and again until stop criteria is met.

### Syntax

```py
repeat { 
   #code
   if(condition) {
      break
   }
}
```
### Example

```py
i <- 1
repeat {
   print(i)

   if ( i == 5){
     break
   }
   i = i+1;   
}
```
#### Check result [here](https://onecompiler.com/r/3vs9ugn5r)

