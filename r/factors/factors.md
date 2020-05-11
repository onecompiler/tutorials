Factor is a data object which is used to take a limited number of different values and categorize them into multiple levels. They can take both strings and integers. 

They have labels which are associated with the unique integers stored in it and also contains level which are predefined set value.

`factor()` function is used to convert a vector into factor.

```py
factorData<- factor(inputVector)
```
`is.factor()` is used to check whether the input given is a factor or not.

```py
is.factor(factorData)
```

### Example

```py
inp <- c("MALE", "FEMALE","MALE","FEMALE","RATHER NOT SAY")  
  
print(inp)  
print(is.factor(inp))  
  
factor_inp<- factor(inp)  
  
print(factor_inp)  
print(is.factor(factor_inp)) 
```
### Check Result [here](https://onecompiler.com/r/3vsf268zr)

## How to access factors

Factors can be accessed by using it's index.

```py
inp <- c("MALE", "FEMALE","MALE","FEMALE","RATHER NOT SAY")  

factor_inp<- factor(inp)  
print(factor_inp)  

print(factor_inp[1])
```
### Check Result [here](https://onecompiler.com/r/3vsf2rnca)