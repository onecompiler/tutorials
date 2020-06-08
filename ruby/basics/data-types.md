As the name suggests, data-type specifies the type of the data. There are different built-in datatypes in Ruby:

## 1. Numbers

Numbers represents both Integers and floating point numbers.

| Class | Description | Example |
|----|----|----|
| Fixnum |	Represents small integers |	99|
| Bignum |	Represents big intergers | 999999999 |
| Float	| Represents decimal numbers| 7.9 |
| Complex | Represents imaginary numbers |	5 + 2i |
| Rational | Represents fractional numbers |	10/3 |
| BigDecimal | Represents Precision decimal numbers |	79.2|

## 2. Boolean

Boolean data type represents either true or false.

## 3. Strings

String represents a series of characters. Strings can be enclosed with in either single quotes, or double quotes.

```ruby
str1 = "hello World";
str2 = 'good morning';
```

## 4. Hashes

Hashes represents key-value pairs. `=>` is used to assign value to it's key. 

```ruby
hsh = nationalGame = { "Australia" => "Cricket", "Japan" => "Wrestling", "NewZealand" => "Rugby","USA" => "Baseball"}
```
## 5. Arrays

Array is a collection of data items. They don't need to be of same type. It can contain all different types of data.

```ruby
arr = [ "Good", "morning", 9, 5.32 , true]
```

## 6. Symbols

Symbols are used instead of strings because they are light-weight strings and occupy less memory. A symbol is preceded by a colon (:). 

```ruby
:oc => "OneCompiler"
```

