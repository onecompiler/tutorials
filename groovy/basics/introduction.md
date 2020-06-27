Apache Groovy is a dynamic and agile language which is similar to Python, Ruby, Smalltalk etc. It's a powerful, optionally typed and dynamic programming language which is based on Java. 

## Key Features

* It's not a replacement for java but it's an enhancer to Java with extra features like DSL support, dynamic typing, closures etc.
* Accepts Java code as it extends JDK and can integrate smoothly with any java program.
* Greater flexibility
* Concise and much simpler compared to Java
* Can be used as both programming language and scripting language.

# Installation

## 1. On Windows

1. Download the software from [Groovy Downloads](http://www.groovy-lang.org/download.html)
2. Double click on the exe file and the setup will guide you through the installation process and any settings if required.
3. run the command `groovysh` in the command prompt to check if the installation is done correctly. 

## 2. On Mac OSX, Linux, Cygwin, Solaris or FreeBSD

1. Installation of Groovy is very easy with the help of SDKMAN( Software Development Kit Manager).
2. Open terminal and execute the below command

    ```sh
    curl -s get.sdkman.io | bash
    ```
3. Open another new terminal and type the below command:

    ```sh
    $ source "$HOME/.sdkman/bin/sdkman-init.sh"
    ```
4. To install the latest stable Groovy:
    ```sh
    $ sdk install groovy
    ```
5. Once installation is complete, test using the below command.
    ```sh
    $ groovy -version
    ```
## 3. Using OneCompiler

1. You don't need to install any software or compiler.
2. Just goto [OneCompiler](https://onecompiler.com/groovy) and choose the programming language as `Groovy` and enjoy programming without any installation.

# Sample Program

```java
println "Hello World!"
```
### Try yourself [here](https://onecompiler.com/groovy)

The above program when written in Java:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
         }
}
```

As you can see Groovy is much simpler and concise. You don't need to worry on curly braces or paranthesis and no need to write your program in main(). Even the print statement is much concise. This is because Groovy internally converts the script into the program similar to java program with class and main().
 