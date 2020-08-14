* Iterator is an interface that is available in the Java collection framework which is in java.util package.
* It is used to iterate over a collection of objects.
* It will traverse the collection of elements one by one
* It has been added to Java in version 1.2
* It has Read and Remove operations

## Iterator Methods

* boolean hasNext(): return true if there is a element after current element else false
* Object next(): Returns the next element. if there are nothing throws an exception
* void remove(): Removes current element.

### Example

```java

import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

public class ExternalIteratorDemo {

    public static void main(String[] args) {

        List<String> cities = new LinkedList<>();

        cities.add("chennai");
        cities.add("kolkata");
        cities.add("banglore");
        cities.add("vizag");

        Iterator<String> cityIterator = cities.iterator();

        while(cityIterator.hasNext()){
        System.out.println(cityIterator.next());
        }
    }
}

```

check result [here](https://onecompiler.com/java/3w3vnankp)
