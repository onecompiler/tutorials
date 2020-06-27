Javascript is a dynamically typed language and hence though there are data types, variables are not bound to them. In short, a variable can hold any type of data. Same variable can be a number or string or boolean or any other value. Javascript will not throw any error even if you assign a string value to integer variable.

There are 7 different types of data types present in Javascript.

## 1. Number

Number datatype represents both integers and floating values. Also, it represents Infinity and NaN values as well. 

### Example
```javascript
let x = 999;
```

## 2. String

String datatype represents text or you can say series of characters. You can use double quotes (" ") or single quotes (' ') or back ticks (\`\`) to represent strings.

### Example
```javascript
// all the three are correct
let x ="OneCompiler";
let y ='OneCompiler';
let z =`OneCompiler`;
```

## 3. BigInt 

BigInt datatype is used to represent arbitrary length integers as there is a technical limitation for number datatype that it can not represent larger than 9,007,199,254,740,992.

### Example
```javascript
let x = 99999999999999999;
```

## 4. Null

Null is a special value which is used to represent `nothing` or `no value` or `unknown value`.

### Example
```javascript
let x = null;
```

## 5. Undefined

Undefined is also a special value which is used to represent `value is not assigned`. This is predominantly used to check if a variable is assigned or not.

### Example
```javascript
let x;
console.log(x);
```

## 6. Boolean

Boolean datatype represents boolean values: true or false.

### Example
```javascript
let x = true;
```

## 7. Object

All the above six datatypes are called primitive datatypes as they contain some value. Object datatype represents complex entities which consists of key-value pairs. Objects can be created using curly brackets({ }).

### Example
```javascript
let employee = {  // object
     "name" : "foo", //object property
     "last name" : "bar", //object property
     "id"   : 123 //object property
};
```
Let's us see in-detail about Objects in further topics.

## Summary

|Data Type | Description |
|----|----|
|number| Represents numbers like integers, floating-values etc|
|string| represents one or more characters|
|bigint| represents integers of arbitrary length|
|null| represents unknown values|
|undefined| represents undefined values|
|object| represents complex data structures|
|boolean| represents either true or false

