# Fundamentals of C++ Programming

### § Introduction

_ 1.1 Hello World! _

C++ is a compiled language. Source text is processed by a compiler, producing object files, which are combined by a linker to yield an executable program. An executable program created in C++ is not usually portable for different operating systems. When discussing the portability of C++ programs, we usually mean the portability of source code. 

The ISO C++ standard defines two kinds of entities:
* *Core language features*, such as built-in types (e.g., _char_ and _int_) and loops (e.g., _for_ statements)
* *Standard-library components*, such a containers (e.g., _vector_ and _map_) and I/O operations (e.g., _<<_ and _getline()_)

C++ is a statically typed language. That is, the type of every entity (e.g., object, value, name, and expression) must be known to the compiler at its point of use. The type of an object determines the set of operations applicable to it. 


A minimal *hello world* example is as follows:

```C++
#include <iostream>

int main() {
  std::cout << "Hello World!\n";
}
```

Every C++ program must have exactly one global function named *main()*. The int value returned by main(), if any, is the program's return value to the system. If no value is returned, the system will receive a value indicating successful completion. A nonzero value from main() indicates failure.

The line #include <iostream> instructs the compiler to include the declarations of the standard stream I/O facilities as found in _iostream_. The std:: specifies that the name cout is to be found in the standard-library namespace. 
  
_ 1.2 Functions _
  
  

