* It is very important to understand Objects in depth in order to understand Javascript as almost everything can be considered as Objects in Javscript.

* All Javascript values except primitives(string, number, bigInt, boolean, null, undefined) are Objects like Arrays, Regular expressions, Functions, Dates, Maths etc.

* Even strings, numbers and booleans can be consisdered as Objects if they are defined with new keyword.

* As discussed before, Object datatype represents complex entities which consists of key-value pairs. Objects can be created using curly brackets({ }) with an optional list of properties.

* A property is a “key: value” pair, where key is a string, and value can be anything. Keys are also called property names.

## Example

```javascript
let employee = {  // object 
     "name" : "foo", // key is name and value is foo
     "id"   : 123 //key is id and value is 123
};
```
In the above example, employee is an object and it has two properties:

1. Property name / key is "name" and it's value is "foo"
2. Property name / key is "id" and it's value is "123".


## How to add a property to the existing Object

Property values can be added using the dot notation like below, if the key name is a multi-word then use [""].

```javascript
employee.isTrainee = true;
employee.businessUnit = "IT"; 
employee["last name"] = "bar"; // [] are used if the property name is a multi-word.
```
## How to access property values

Property values can be accessed using the dot notation like below:

```javascript
console.log(employee.name);
console.log(employee["last name"]); // [] are used if the property name is a multi-word.
```

## How to remove a property from existing Object

```javascript
delete employee.businessUnit;
```
Try yourself [here](https://onecompiler.com/javascript/3vhrvjmm4).

## How to iterate over an Object Properties.

The better way to loop over Objects in Javascript is by converting them to arrays and use normal Array looping techniques.

You can convert an object to an array by using the following methods

### 1. Object.keys
   
   This creates an array which contains the `properties` or `keys` of an object.

### 2. Object.values
   
   This creates an array which contains the `values` of the properties or keys of an object.

### 3. Object.entries
   
   This creates an array which contains the `both` keys and values of an object.

```javascript
let info = {
  name: "foo",
  id: 123
}

const keys = Object.keys(info);
console.log(keys) ;  // [ 'name', 'id' ]

const values = Object.values(info);
console.log(values) ;  // [ 'foo', 123 ]

const arr = Object.entries(info);
console.log(arr) ;  // [ [ 'name', 'foo' ], [ 'id', 123 ] ]
```
### Run [here](https://onecompiler.com/javascript/3vr49n5ds)

The above example helps you to understand how you can convert objects to arrays. If you are using `Object.entries` then you need to destructure it like `const [name,id] of arr`.

Once the objects are converted to arrays, then you can use the normal iteration techniques for an array as discussed [here](https://onecompiler.com/posts/3vgf2g9ag/different-ways-of-iterating-over-an-array-in-javascript).