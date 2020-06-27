C# is a general purpose object-oriented programming language by Microsoft. Though initially it was developed as part of .net but later it was approved by ECMA and ISO standards.

You can use C# to create variety of applications, like web, windows, mobile, console applications and much more using Visual studio.

## Key Features

* One of the popular programming languages.
* Highly expressive syntax still easy to learn.
* Very good community support.
* Simplifies many of C++ complexities and provides powerful features like nullable types, enumerations, delegates, lambda expressions, and direct memory access. 
* Object oriented programming language
* Runs on .net framework
* Ranks #5th best programming language according to [TIOBE INDEX](https://www.tiobe.com/tiobe-index/)
* Used to make interactive websites, mobile apps, video games, virtual reality etc.

# Installation

## 1. Installation on Windows

Use Microsoft Visual Studio to  develop C# programs, Visual studio is an integrated development environment (IDE) from Microsoft.

1. Download Visual studio [here](https://www.visualstudio.com/downloads/)
2. You can download Visual studio community version which is free IDE for students and open-source developers.
3. Run the executable file and go through the next installation screens and provide neccesary input and finish instllation.

## 2. Installation on Linux

1. Install the .NET Core SDK by using below commands:

```sh
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31 -y
scl enable rh-dotnet31 bash
```
2. Install the .NET Core Runtime by using below commands:

```sh
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31-dotnet-runtime-3.1 -y
scl enable rh-dotnet31 bash
```
## 3. Using Onecompiler

* You don't need to install any software or compiler.
* Just goto [OneCompiler](https://onecompiler.com/csharp) and choose the programming language as `C#` and enjoy programming without any installation.

# Sample program

```c#
using System;

namespace HelloWorld
{
	public class Program
	{
		public static void Main(string[] args)
		{
			Console.WriteLine("Hello, World!");
		}
	}
}
```
* **Using Keyword** : using keyword is used to include namespaces in the program. 
* **Namespace declaration** : Namespace is a container for classes and other namespaces. The `HelloWorld` namespace contains the class `Program`.
* **Public Class** : class is a container for data and methods, you are declaring `Program` as a class with public visibility.
* **Main** : Beginning of your program
* **Console.WriteLine** : Console is a class of the System namespace and WriteLine() is a method in it which is used to print text to the console.
* C# statements end with a semicolon `;`
* C# is Case-sensitive
* `//` : Single line Comment
* `/* */` : Multi Line Comments
