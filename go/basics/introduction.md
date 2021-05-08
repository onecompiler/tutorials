Go language is an open-source, statically typed programming language by Google. Go is highly recommended for creation of highly scalable and available web applications. 

Some of the products developed by using Go are Kubernetes, Docker, Dropbox, Infoblox etc.

## Key Features
* Fast compilation
* Concurrency is maintained easily using Go-routines.
* Simple and concise syntax
* Supports static linking
* Open-source and huge community support.
* High performance
* Uses interfaces as building blocks for code re-usability.

## Why you should learn Go?
* Modern programming language by Google
* Easy to write concurrent programs
* There is good documentation available.
* Go comes with a built-in testing tool 
* Easy to learn as the syntax is simple and easy.

## Why Go is popular?

* Go is one of the most popular and fastest growing languages recently.
* Go is popular because of it's concurrency efficiency 
* Simple and concise syntax making it easy for developers
* Fast compilation speed
* More popular in the system and infrastructure space. 
* Google using this language making it more popular.

# Go Basics

## Sample Go program

```c
package main
import "fmt"

func main() {
	fmt.Printf("Go Hello, World!")
}
```
### Run your program [here](https://onecompiler.com/go)

Let's look into each line of the above sample program:

* **`package`** : Used to declare packages which is mandatory for all Go programs. 
* **`import "fmt"`**: is used to import built-in fmt package.
* **`func main()`** : it is the starting point of execution of any Go program.
* **`fmt.printf`**: it's an inbuilt library function which is used to print the given message.


## Comments

1. Single line comments

    `//` is used to comment a single line

2. Multi line comments

    `/**/` is used to comment multiple lines.

 
## Installation

## 1. On MacOS, Linux and Unix systems

* Download the software from [Go Downloads](https://golang.org/dl/)

* Execute the below command after replacing * with filename you have downloaded.
    ```sh
    tar -C /usr/local -xzf *.tar.gz  
    ```
* Add `/usr/local/go/bin` to PATH environment variable.
* If you set any other directory during installation then add the corresponding directory to the path variable.

## 2. On Windows

* Download the software from [Go Downloads](https://golang.org/dl/)
* Execute the *.msi file and follow the installation instructions
* Installation will be done in the path c:\Go by default.
* Add c:\Go\bin directory to Window's PATH environment variable. 
* If you set any other directory during installation then add the corresponding directory to the path variable.

## 3. Using Onecompiler

* You don't need to install any software or compiler.
* Just go to [OneCompiler](https://onecompiler.com/) and choose the programming language as `Go` and enjoy programming without any installation.
