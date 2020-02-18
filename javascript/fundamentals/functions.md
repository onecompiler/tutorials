A function is a block of code which can be re-used multiple times. 

## Syntax
```javacript
function function-name(){
    //code
}
```
## Example

```javascript
function setName(name = "NA"){ // defaulting values to parameters
	return name;
};
console.log(setName()); // NA
console.log(setName("foo")); // foo

func = function({x = 10,y = 11,z = 12} = {x:1,y:2,z:3}) {
    console.log(x,y,z);
};

func(); //1 2 3  (hits the object literal default)
func({}); //10 11 12   (hits the value defaults)
```
### Practice your understanding [here](https://onecompiler.com/javascript) 


### Quick take away:

* A function can be assigned to a variable.
* You can default values to the parameters of a function
* A function is created predominently for re-usability.