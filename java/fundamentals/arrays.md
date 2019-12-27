Array is a collection of similar data which is stored in continuous memory addresses. They are used to store multiple values in a single variable and its values can be fetched using index. 

Index starts from 0 to size-1.

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

// below is also valid and can be used instead of for loop

for (string i : mobiles) {
  System.out.println(i);
}

```

## Practice with more examples [here](https://onecompiler.com/java)