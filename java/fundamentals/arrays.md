Array is a collection of similar data which is stored in continuous memory addresses. They are used to store multiple values in a single variable and its values can be fetched using index. 

You can declare an array by adding square brackets to the datatype. 

## Syntax

```java
data-type[] array-name; // one dimensional array
data-type[][] array-name; // two dimensional array
```
## Example

```java
String[] mobiles = {"iPhone", "Samsung", "OnePlus"};
for (int i = 0; i < mobiles.length; i++) {
  System.out.println(mobiles[i]);
}

// below is also valid and can be used instead of traditional for loop

for (string i : mobiles) {
  System.out.println(i);
}

```

## How to access Array elements

Array elements can be accessed using index. Index starts from 0 to size-1.

```java
public class MyClass {
  public static void main(String[] args) {
        String[] mobiles = {"iPhone", "Samsung", "OnePlus", "Nokia"};
    System.out.println(mobiles[1]);
  }
}
```
### Check result [here](https://onecompiler.com/java/3vk37b86f)

## How to change a particular element in an array

To change a specific element in an array, refer it's index. 


```java
public class MyClass {
  public static void main(String[] args) {
        String[] mobiles = {"iPhone", "Samsung", "OnePlus", "Nokia"};
        mobiles[2] = "Oppo";
        for (int i = 0; i < mobiles.length; i++) {
           System.out.println(mobiles[i]);
        }
  }
}
```
### Check Result [here](https://onecompiler.com/java/3vk37wdrv)

what will happen if you refer an index which is greater than array length?

```java
public class MyClass {
  public static void main(String[] args) {
        String[] mobiles = {"iPhone", "Samsung", "OnePlus", "Nokia"};
        mobiles[4]= "Vivo";
        for (int i = 0; i < mobiles.length; i++) {
           System.out.println(mobiles[i]);
        }
  }
}
```
### Find out [here](https://onecompiler.com/java/3vk38cj9f)

# Exercises

## 1. How do you convert an Array to String?

## 2. How can you determine length or size of an Array in Java?

## 3. How do you add an element to an Array

## 4. How to check if a value is present in an array?

## 5. How to convert vector to array?

## 6. How to convert linkedlist to array?

## 7. How to compare two arrays?
