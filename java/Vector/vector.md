The Vector class is used to initialize a dynamic array.In other words an array whose length is not fixed and we can shorten or increase the length of it.

We can declare Vectors in multiple ways :

## Syntax

 *E* is referred to the type of element
```java
 Vector<E> v = new Vector<E>();                      //creates default vector of size 10       
 Vector<E> v = new Vector<E>(int size);              //creates vector of given "size"

 Vector<E> v = new Vector<E> (int size,int inr);     /*creates a vector of given "size" and the capacity of vector increases by "inr"
                                                       each time the vector is resized upward*/ 
```     

## Initializing , Adding and Printing

```java
import java.util.*;
public class vector
{
    public static void main()
    {
        Vector<String> vec = new Vector<String>(4);    //create a vector of size 4
        vec.add("One");
        vec.add("Two");
        vec.add("Three");  
        vec.add("Four");
        System.out.println("Elements are : "+vec);
    }
}
```

### Check Result [Here](https://onecompiler.com/java/3yjdwnnwr)

