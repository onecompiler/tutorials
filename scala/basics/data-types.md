As the name suggests, data-type specifies the type of the data present in the variable.

Scala supports all types of data types available in Java. Some of the most commonly used data types are as follows:

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
|string | used to store strings|sequence of characters|2bytes per character|

# Literals

Literals/constants are used to represent fixed values which can't be altered later in the code.

## 1. Integer literals

Interger literals are numeric literals. Below are the examples of various types of literals

```c
79         // decimal (base 10) 
0253       // octal (base 8)
0x4F       // hexadecimal (base 16)
22         // int
53u        // unsigned int
79l        // long
7953ul       // unsigned long
```

## 2. Float point literals

Float point literals are also numeric literals but has either a fractional form or an exponent form.

```c
79.22         // valid
79.2539f        // valid
53e          // not valid as it is incomplete exponent
1.0e22          // valid 
```

## 3. Boolean literals

There are two Boolean literals:

* true value representing true.

* false value representing false

## 4. Character literals

Character literals are represented with in single quotes. For example, 'a', '1' etc. A character literal can be a simple character (e.g., 'a'), an escape sequence (e.g., '\n'), or a universal character (e.g., '\u02C0').

|Escape sequence| Description|
|----|----|
|\n	| New line|
|\r	| Carriage Return|
|\?	| Question mark|
|\t	| Horizontal tab|
|\v	| Vertical tab|
|\f	|Form feed|
|\\	| Backslash|
|\'	| Single quotation|
|\"	| Double quotation|
|\0 | Null character|
|\? | ? Question mark|
|\b	|Back space|
|\a | alert or bell|

## 5. String literals

String literals are represented with in double quotes. String literals contains series of characters which can be plain characters, escape sequence or a universal character.

```c
"Hello World"
```

