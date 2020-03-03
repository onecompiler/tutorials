Variables are like containers which holds the data values. A variable specifies the name of the memory. location. 

| Types | Description |
|-----|------|
| Local Variable | Declared inside the body of a method. Life of the local variable will be only with in the method it is declared|
| Instance Variable| Declared inside the class but outside methods. These are called instance variables because the values of the instance variables are instance specific and are not shared with other instances.|
| Static Variable| These are declared with static keyword. | 

## How to declare variables

```java
data-type variable-name = value;
```

### Example

```java
Class Sum {
    int n1 = 10; // Instance  Variable
    static int n2 = 20; //static variable
    void sum(){
        int n3 = 30; //local variable
        int total = n1+n2+n3;
    }
}
```
