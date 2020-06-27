As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

There are two groups of Data types in Java.

## i. Primitive data types

Primitive data types specifies the type and size of the data present in variables and they don't have additional methods.

| Data type | Description | Range | Size|
|---|---|---|---|
| int| used to store whole numbers|-2,147,483,648 to 2,147,483,647|4 bytes|
|short| used to store whole numbers|-32,768 to 32,767| 2 bytes|
|long| used to store whole numbers|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807| 8 bytes|
|byte| used to store whole numbers|-128 to 127| 1 byte|
|float| used to store fractional numbers|6 to 7 decimal digits| 4 bytes|
|double| used to store fractional numbers|15 decimal digits| 8 bytes|
|boolean| can either store true or false |either true or false| 1 bit|
|char|used to store a single character|one character|2 bytes|

### Examples

#### 1. Integer data type 

`int` is used to store whole numbers from -2147483648 to 2147483647.

```java
int x = 99999; 
System.out.println(x);
```

#### 2. Short data type 

`short` is used to store whole numbers from -32768 to 32767

```java
short x = 999; 
System.out.println(x);
```

#### 3. Long data type 

`Long` is used when int is not enough to hold the data and can store whole numbers from -9223372036854775808 to 9223372036854775807. You should put `L` at the end of the value.

```java
long x = 99999999999L;
System.out.println(x);
```

#### 4. Byte data type 

`Byte` can store whole numbers from -128 to 127. This data type is used to save memory and is used when the value is with in -128 to 127.

```java
byte x = 99;
System.out.println(x);
```

#### 5. Double data type 

`Double` is used store fractional numbers in the range 1.7e−308 to 1.7e+308. You should put `d` at the end of the value.

```java
double x = 99.99d;
System.out.println(x);
```
#### 6. Boolean data type 

`Boolean` data type is used only when the value is either true or false. 

```java
boolean isAvailable = true;
System.out.println(isAvailable);
```
#### 7. Char data type 

`char` data type is used to store single character. Single quotes are used to represent characters.

```java
char division = 'A';
System.out.println(division);
```

## ii. Non-Primitive data types

Non-primitive data types specifies the complex data values and it can have additional methods. Usually non-primitive data types are created by the developer (except for String).

For example, strings, arrays and classes can be referred as Non-primitive data types and are discussed in later sections.


