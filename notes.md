# Fundamentals of C++ Programming

### § Introduction

**1.1 Hello World!**

C++ is a compiled language. Source text is processed by a compiler, producing object files, which are combined by a linker to yield an executable program. An executable program created in C++ is not usually portable for different operating systems. When discussing the portability of C++ programs, we usually mean the portability of source code. 

The ISO C++ standard defines two kinds of entities:
* *Core language features*, such as built-in types (e.g., **char** and **int**) and loops (e.g., **for** statements)
* *Standard-library components*, such a containers (e.g., **vector** and **map**) and I/O operations (e.g., **<<** and **getline()**)

C++ is a statically typed language. That is, the type of every entity (e.g., object, value, name, and expression) must be known to the compiler at its point of use. The type of an object determines the set of operations applicable to it. 


A minimal *hello world* example is as follows:

```C++
#include <iostream>

int main() {
  std::cout << "Hello World!\n";
}
```

Every C++ program must have exactly one global function named **main()**. The int value returned by main(), if any, is the program's return value to the system. If no value is returned, the system will receive a value indicating successful completion. A nonzero value from main() indicates failure.

The line #include <iostream> instructs the compiler to include the declarations of the standard stream I/O facilities as found in _iostream_. The std:: specifies that the name cout is to be found in the standard-library namespace. 
  
**1.2 Functions and Fundamental Types**

A function declaration gives the name of a function, the type of value returned (if any), and the number and types of arguments that must be supplied in a call. The argumen types are checked and implicit argumentt ype conversion takes place when necessary at compile-time. A function declaration may contain argument names, but is not required. The type of a function consists of the return type and the argument types. For class-member functions, the naem of the class is also part of the fuction type. 

```C++
double get(const vector<double>& vec, int index); // type: double(const vector<double>&, int)
char& String::operator[](int index); // type: char& String::(int)
```

C++ offers a variety of fundamental types. For example, bool, char, int, double, unsigned. Each fundamental type corresponds directly to hardware facilities and has a fixed-size that determines the range of values that can be stored in it. 

C++ offers a variety of notations for expressing intializations, such as **=** and a universal form based on curly-brace-delimited initializer lists:

```C++
double d1 = 2.3;
double d2 {2.3};
```

The **=** is traditional and dates back to C, but if in doubt, use the general {}-list form. If nothing else, it saves you from conversions that lost information!

```C++
int i1 = 7.2; // i1 becomes 7
int i2 {7.2}; // error: floating-point to integer conversion.
int i3 = {7.2}; // error: floating-point to integer conversion (the = is redundant)
```

Unfortunately, conversions that lose information (*narrowing conversions*), suc as double to int and int to char are allowed and implicility applied. The problems caused by implicit narrowing conversions is a price paid for C compatability. 

When defining a variable, you don't need to state its type explicitly when it can be deduced from the initializer:

```C++
auto b = true; // a bool
auto ch = 'x'; // a char
```

With **auto**, we use the **=** because there is no potentially troublesome type conversion involved. We use **auto** when we don't have a specific reason to mention the type explictly (and is useful in generic programming).

**1.3 Scope and Lifetime**

A declaration introduces its name into a scope:
* Local scope: A name declared in a function, or lambda, is called a *local name*. A block is delimited by a {} pair. Function names are considered local names. Its scope extends from its point of declaration to the end of the block.
* Class scope: A name is called a *member name* (or *class member name*) if it is defined in a class, or enum class. Its scope extends from the opening { of its enclosing declaration to the end of that declaration.
* Namescape scope: A name is called a *namescape member name* if it is defined in a namespace. 
* Global scope: A name not declared inside any other construction is a *global name* and is in the *global namespace*

In addition, objects are created using the **new** keyword. An object must be constructed (intitialized) before it used and will be destroyed at the end of its scope. For a namespace object, the point of destruction is the end of the program. For a member, the point of destruction is determined by the point of destruction of the object of which it is a member. An object created by **new** "live" until destroyed by **delete**. 

**1.4 Constants**

C++ supports two notions of immutability:
* **const**: meaning roughly "*I promise not to change this value*". This is used primary to specify interfaces, so that data can be passed to functions without fear of it being modified. The compilre enforces this.
* **constexpr**: meaning roughly "to be evaluated at compile time". This is used primarily to specify constants, to allow placement of data in read-only memory, and for performance.

For example,

```C++
const int dmv = 17; // dmv is a named constant
int var = 17; // var is not a constant

constexpr double max1 = 1.4*square(dmv); // OK if square(17) is a constant expression
constexpr double max2 = 1.4*square(var); // error: var is not a constant expression
const double max3 = 1.4*square(var); // OK, may be ealuated at runtime.

double sum(const vector<double>&); // sum will not modify its argument
vector<double> v {1.2, 3.4, 4.5}; // v is not a constant
const double s1 = sum(v); // OK: evaluated at runtime
constexpr double s2 = sum(v); // error: sum(v) is not a constant expression.

```

For a function to be usable in a constant expression, that is, in an expression that will be evaluated by the compiler, it must be defined constexpr. For example,
```C++
constexpr double square(double x) { return x*x; }
```

To be a constexpr, a function must be rather simple, just a return statement computing a value. A constexpr function can be used for non-constant arguments, but when that is done the result is not a constant expression. 

**1.5 Pointers, Arrays and References**

An array of elements can be declared as:
```C++
char v[6];
```

Similarily, a pointer can be declared like this:
```C++
char* p;
```

In declarations, [] means "array of", and * means "pointer to". All arrays have 0 as their lower bound. The size of an array must be a constant expression. A pointer variable can hold the address of an object of the appropriate type.
```C++
char* p = &v[3]; // p points to v's fourth element.
char x = *p; // *p is the object that p points to (dereference operator)
```

A prefix unary * means "contents of" and a prefix unary & means "address of". References are particular useful for specifying function arguments. For example,
```C++
void sort(const vector<double>& v); //sort v
```
By using a reference, we ensure that for a call sort(my_vec), we do not copy my_vec and that it is really is my_vec that is sorted and not a copy of it. When we don't want to modify an argument, and still don't want the cost of copying, we use a const reference. 

## § User-defined types
  
  
  

