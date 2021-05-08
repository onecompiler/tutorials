Java is a very popular programming language. Java can be used to develop anything and almost everything like web applications, web servers, application servers, mobile applications and so on. 

## Key Features

* Very popular and widely used programming language.
* works on different platforms.
* open-source and free as well
* tremendous community support
* follows OOPS
* Compilation of java programs return byte code which can be run on any platform using JVM(java virtual machine).
* Ranked as #1 programming language according to [TIOBE 2020 Index](https://www.tiobe.com/tiobe-index/)

## Why you should learn Java

* Java is the most commonly used language and it's always good to know Java concepts to learn other programming languages. 
* Java developers are offered with high salaries.
* It has very large community support
* A number of open-source libraries
* easy to learn language.
* Powerful development IDE's like Eclipse, NetBeans, IntelliJ IDEA, etc.
* It's free of cost and platform independent.


## Why Java is so popular?

* Java is in market from almost 25 years.
* 3 billion devices run on Java like ATM's, Routers, Switches, SmartCards, Printers, Sensors and many other devices.
* Fully object oriented programming language
* Platform independent and hence run anywhere like mobiles, Windows, linux, MacOS based machines or mainframe systems literally anywhere as long as JRE is in place. 
* More secure

## Installation

## On Windows

1. Download the software from [Oracle Downloads](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
2. Run the executable file to install JDK
3. Set the PATH of Java installation path in the environment variables.

## On Linux
1. Download the software from [Oracle Downloads](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
2. Run the executable file to install JDK
3. Run the below command
```sh
sudo tar -xvf jdk*.tar.gz
```
4. Add System variables at the end of `/etc/profile` file
```sh
JAVA_HOME=<Java Extracted Directory>/jdkversion
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export JAVA_HOME
export PATH
```

## Using OneCompiler

1. You don't need to install any software or compiler.
2. Just goto [OneCompiler](https://onecompiler.com/) and choose the programming language as `Java` and enjoy programming without any installation.

## Sample Program

```java
import java.util.Date;

public class HelloWorld {
    public static void main(String[] args) {
        Date now = new Date();
        System.out.println("Hello World!");
        System.out.println("now: " + now);
    }
}
```
#### Check Result [here](https://onecompiler.com/java)

### How to run your program

#### 1. Using Onecompiler

* Go to [OneCompiler](https://onecompiler.com/java). You can also add dependent packages here and run your code Online with out any installation. There are thousands of sample programs which you can look for reference.

#### 2. Using command prompt

* Go to Command prompt and navigate to the folder where java files are stored.

### To Compiler Java program

```cmd
javac example.java
```
### To Run Java program

```cmd
java example 
```
