* Java Annotations are used to add metadata information into our code
* Annotations doesn't affect the execution of the program

## Built-in Annotations in Java
Java has three built-in Annotations. They are :
* @Override
* @Deprecated
* @SupressWarnings

## 1. @Override
* We should use this annotation while overriding the child class method
### Example
```java
class A {
  public void show () {
    System.out.println("Class A");
  }
}

class B extends A {
  
  @Override
  public void show () {
    System.out.println("Class B");
  }
  public static void main (String[] args) {
    B ob = new B();
    ob.show();
  }
}
```
### check output [here](https://onecompiler.com/java/3w4fga8at)

## 2. @Deprecated
* Used to mark elements that are no longer used
* Compiler gives a warning when Deprecated elements are used
### Example
```java
class Example {
  void read2 () {
    System.out.println("updated method");
  }
  
  @Deprecated
  void read1 () {
    System.out.println("outdated method");
  }
}

class Main {
    public static void main(String[] args) {
        Example ob = new Example();
        ob.read2();
        ob.read1();
    }
}
```
### check output [here](https://onecompiler.com/java/3w4fgjd3e)

## 3. @SupressWarnings
* Used to tell the compiler, to ignore specific warnings

### Example

```java
class Example { 
	@Deprecated
	public void show() { 
		System.out.println("outdated function"); 
	} 
} 

class Main { 
	@SuppressWarnings({"checked", "deprecation"}) 
	public static void main(String args[]) { 
		Example ob = new Example(); 
		ob.show(); 
	} 
} 
```

## Built-in Java Annotations used in other annotations

### 1. @Target 
Used to specify at which type, the annotation should be used

### 2. @Retention
Used to specify the level at which the annotation is available

### 3. @Inherited
Marks the annotation should be inherited to the subclasses

### 4. @Documented
Used to tell an element should be Documented by JavaDoc