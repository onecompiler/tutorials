Scala is both object-oriented and functional programming language which was designed by Martin Odersky.

## Key Features

* Static typed language
* You can create any kind of application using Scala like web applications, mobile applications, enterprise applications etc.
* You are not required to mention data type for variables and return type for functions explicitly.
* Variables are immutable once declared
* Evaluates expressions only when required
* Concurrency control
* Provides rich set of collection libraries.
* Case sensitive

# Installation

**Pre-requisite** : You must have Java 8 JDK (or Java 11 JDK) installed on your machine and also check if your environment variable contains Java path

## Installation on Windows

* Download the software from [Scala Downloads](https://www.scala-lang.org/download/)
* If you want a IDE based one, download IntelliJ else if you want command line based one, go for download SBT.
* Run the executable file to install Scala
* Follow the installation steps by providing path etc and finish installation.

## Installation on Linux

* Download the software from [Scala Downloads](https://www.scala-lang.org/download/)

* Install scala using the below command:

```sh
sudo apt-get install scala
```

## Using Onecompiler

1. You don't need to install any software or compiler.
2. Just goto [OneCompiler](https://onecompiler.com/scala) and choose the programming language as `Scala` and enjoy programming without any installation.

## Sample program

```java
object HelloWorld {
	def main(args: Array[String]): Unit = {
	println("Hello, World!")
	}
}
```
* ***object*** :  Object is a instance of a class. In Scala, everything is considered as objects.
* ***def*** : def is used for function declaration
* ***main*** : entry point of the program
* ***println*** : prints data to the console.
* `//` : Single line comment
* `/*` Multi
    Line
    comment `*/`
