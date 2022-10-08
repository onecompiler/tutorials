## Final Keyword in JAVA

Final keyword in java is a non-access modifier. It is used to restrict user from making any change in variable, method and classes.

### 1. Final Variable
If any variable is declared with keyword final, then the value of final variable can't be change, i.e. it becomes constant.

### Example

```java
class Student{
    //final variable
    final int id=50;
    void get_id(){
        id=100;
    }
    public static void main(String[] args){
        Student obj=new Student();
        obj.get_id();
    }
}
```

### 2. Final Method
If any method is declared with keyword final, then it can't be overridden.

### Example

```java
class Fruit{
    //final method
    final void run(){
        System.out.println("Inside Fruit Class");
    }
}
class Apple extends Fruit{
    void run(){
        System.out.println("Inside Apple Class");
    }
    public static void main(String[] args){
        Apple obj=new Apple();
        obj.run();
    }
}
```

### 3. Final Class 
If any class is declared with keyword final, then it can't be inherited.

### Example

```java
//final class
final class Fruit{
}
class Apple extends Fruit{
    void run(){
        System.out.println("Inside Apple Class");
    }
    public static void main(String[] args){
        Apple obj=new Apple();
        obj.run();
    }
}
```
