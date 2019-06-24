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

The **=** is traditional and dates back to C, but if in doubt, use the general {}-list form. If nothing else, it saves you from conversions that lose information!

```C++
int i1 = 7.2; // i1 becomes 7
int i2 {7.2}; // error: floating-point to integer conversion.
int i3 = {7.2}; // error: floating-point to integer conversion (the = is redundant)
```

Unfortunately, conversions that lose information (*narrowing conversions*), such as double to int and int to char are allowed and implicility applied. The problems caused by implicit narrowing conversions is a price paid for C compatability. 

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

In addition, objects are created using the **new** keyword. An object must be constructed (intitialized) before it used and will be destroyed at the end of its scope. For a namespace object, the point of destruction is the end of the program. For a member, the point of destruction is determined by the point of destruction of the object of which it is a member. An object created by **new** "lives" until destroyed by **delete**. 

**1.4 Constants**

C++ supports two notions of immutability:
* **const**: meaning roughly "*I promise not to change this value*". This is used primary to specify interfaces, so that data can be passed to functions without fear of it being modified. The compiler enforces this.
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
By using a reference, we ensure that for a call sort(my_vec), we do not copy my_vec and that it really is my_vec that is sorted and not a copy of it. When we don't want to modify an argument, and still don't want the cost of copying, we use a const reference. 

### § User-defined types

**2.1 Structures**

The first step in building a new type is to organize the element into a **struct**
```C++
struct Vector {
  int sz;
  double* elem;
 };
 ```
 
A variable of type **Vector** can be defined and initialized like this:
```C++
Vector v;
void vector_init(Vector& v, int s) {
  v.elem = new double[s];
  v.sz = s;
}
```
The new operator allocates memory from the heap. Objects allocated from the heap are independent of the scope from which they are created and "live" until they are destroyed using the **delete** operato.

We use **.** (dot) to access **struct** members through a name (and through a reference) and **->** to access **struct** members through a pointer. For example,
```C++
void f(Vector v, Vector& rv, Vector* pv) {
  int i1 = v.sz; // access through name.
  int i2 = rv.sz; // access through reference.
  int i4 = pv->sz; // access through pointer.
}
```

**2.2 Classes**

While structs provide the ability of seperating the data from its operations, a tighter connection between the representation and the operations is still needed for a user-defined type to have all the properties of a "real type". In particular, we may want to keep the representation inaccesssible to all users, so as to ease use, guarantee consistent use of data, and allow later improvements. To do that, we have to distinguish between the interface to a type (to be used by all) and its implementation (which has access to otherwise inaccessible data). The language mechanism for that is called a class.

A **class** is defined to have a set of *members*, which can be data, function, or type members. The interace is defined by the **public** members of a class, and **private** members are accessible only through that interface. 

```C++
class Vector {
public:
  Vector(int s): elem{new double[s]}, sz{s} {} // Construct a vector
  double& operator[](int i) { return elem[i]); } // element access via subscripting
  int size() { return sz;}
private:
  double* elem; // pointer to the elements
  int sz; // number of elements
};

// Call via
Vector v(6)
```

Note: There is no fundamental difference between a **struct** and a **class**; a struct is simply a class with members public by default. For example, you can define constructors and other member functions for a struct.

**2.3 Unions**

A **union** is a struct in which all members are allocated at the same address so that the union occupies only as much space as its largest member. Naturally, a union can hold a value for only one member at a time. For example, consider a symbol table entry:

```C++
enum Type{ str, num };

struct Entry {
  char* name;
  Type t;
  char* s; // Use s if t == str
  int i; // Use i if t == num
};

void f(Entry* p) {
  if (p->t == str)
      // Do stuff..
   //...
 }
```

The members **s** and **i** can never be used at the same time, so space is wasted. It can be easily recovered by specifiying that both should be members of a union, like this:

```C++
union Value {
  char* s;
  int i;
};

// Update struct entry
struct Entry {
  char* name;
  Type t;
  Value v; // Use v.s if t == str, v.i otherwise
}
```

Maintaining the correspondence between a type field (here, t) and the type held in a union is error-prone. To avoid errors, one can encapsulate a union so that the correspondece between a union and union members is guaranteed. At the application level, abstractions relying on such *taged unions* are common and useful. 

**2.4 Enums**

In addition to classes, C++ supports enumerations.
```C++
enum class Color {red, blue, green};
enum class Traffic_light {green, yellow, red};

Color col = Color::red;
Traffic_light light = Traffic_light:red;
```

The class after the enum specifies that a enumeration is strongly typed and that its enumerators are scoped. By default, an enum class has only assignment initialization and comparisons, but you can define operators for it.
```C++
Traffic_light& operator++(Traffic_light& t) {
  // Switch traffic lights based on current light.
 }
 
 // Increment traffic light
 ++light;
 ```
 
 ### § Classes
 
 A **class** is a user-defined type provided to represent a concept in the code of a program. 
 
 **3.1 Concrete Types**
 
 The basic idea of a concrete class is that they behave just like built-in types. For example, a vector and a string are much like built-in arrays, except that they have their own semantics and set of operations. 
 
 The defining characteristic of a concrete type is that its representation is part of its definition. In many important cases, such as a vector, the representation is only one or more pointers to data stored elsewhere, but it is present in each object of a concrete class. This allows implementation to be optimally efficient in time and space. In particular, it allows us to:
* refer to objects directly (and not just through pointers on the stack)
* place objects of concrete types on the stack, in statically allocated memory, and in other objects
* initialize objects immediately and completely
* copy objects

The representation can be private and accessible only through the member functions, but it is present. Therefore, if the representation changes in any significant way, a user must recompile. For types that don't change often, and where local variables provide much-needed clarity and efficiency, this is acceptable and often ideal. To increase flexibility, a concrete type can keep major parts of its representation on the free store (dynamic memory, heap) and access them through the part stored in the class object itself. 

```C++
class Complex {
  double re,im;
public:
  complex(double r, double i): re{r}, im{i} {} // construct complex from two scalars
  complex(double r): re{r}, im{0} {} // construct complex from one scalar.
  complex(): re{0}, im{0} {} // default complex: {0,0}
  
  // get and setters
  double real() const { return re; }
  void real(double d) { re=d; }
  double imag() const { return im; }
  void image(double d) { im=d; }
  
  // operators
  complex& operator += (complex z){ re += z.re, im+=z.im; return *this; } // add to re and im
  complex& operator -= (complex z){ re-=z.re, im-=z.im; return *this; } //   and return the result
  complex& operator*= (complex) { ... };
  complex& operator/= (complex) { ... };
};
```

**3.2 Abstract Types**

An abstract type is a type that completely insulates a user from its implementation details. To do that, we decouple the interface from the representationand give up genuine local variables. Since we don't know anything about the representation of an abstract type (not even its size), we must allocate objects on the free store and access them through references or pointers.

We can look at an example of an abstract type known as a **Container**. A container is an object holding a collection of elements (such as a vector). As an aside, we need to update our Container with a *destructor*. A **destructor** is a mechanism tha allows memory allocated on the free store to be deallocated. 

```C++
class Vector {
private:
  double* elem;
  int sz;
 public:
  Vector(int s): elem{new double [s]}, sz{s} { // constructor: acquire resources
    for (int i = 0; i != s; ++i) 
      elem[i] = 0;
   }
   
   ~Vector() {delete[] elem;} // destructor: release resources
   
   double& operator[](int i);
   int size() const;
};
```

The name of a destructor is the complement operator **~** followed by the name of the cla; it is the complement of the constructor. The destructor cleans up by freeing that memory allocated by the array using the delete operator. A destructor provides the user with seemless garbage collection as once the object is out of scope, the deconstructor will be initialized and destroyed. 

Convenient ways exist to initialize containers. An *initializer_list* constructor is a standard-library type known the compiler that understands when we use a {}-list. 

```C++
class Vector {
public:
  Vector(std::initializer_list<double>);
  // ...
  void push_back(double);
  ///
};

// ...

Vector read(istream& is) {
  Vector v;
  for (double d;  is>>d;)
    v.push_back(d);
   return v;
}
```

Note that virtual means "may be redefined later in a class derived from this one". A function declared virtual is called a *virtual function*. A class derived from a Container class provides an implementation for the Container interface. The syntax **=0** syntax says the function is pure virtual; that is, some class derived from Container **must** derive the function. A class with pure virtual function is called an **abstract class**.

```C++
class Container {
public:
  virtual double& operator[](int) = 0;
  virtual int size() const = 0;
  virtual ~Container() {}
};
```

A container that implements the functions by the interface is defined below:

```C++
class Vector_container: public Container {
  Vector v;
public:
  Vector_container(int s): v(s) {}
  ~Vector_container() {}
  
  double& operator[](int i){ return v[i]; }
  int size() const { return v.size(); }
};
```

The **:public** can be read as "is derived from" or "is a subtype of". Class **Vector_container** is said to be derived from class Container, and class Container is said to be a *base* of class **Vector_container**. The members operator[]() and size() are said to *override* the corresponding members in the base class **Container**. 

**3.3 Virtual Functions**

How does the compiler know what method body to bind to a function call? At run time, the compiler convers the name of a virtual function into an index in a *virtual function table* (vtbl). Each class with *virtual functions* has its own vtbl identifying its virtual functions. 

The functions in the *vtbl* allow the object to be used correctly even when the size of the object and the layout of its data are unkown tothe caller. The virtual call mechamism can be made almost as efficient as a "normal function call" mechanism (within 25%). Its space overhead is one pointer in each object of a class with virtual functions plus one vtbl for each such class.










  
  
  
  
  

