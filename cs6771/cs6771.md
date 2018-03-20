# COMP6771 - Advanced C++ Programming

## Week 1
### Style
- NO Macros
- Variables should be defined close to their use
- Don't use `malloc/realloc`, use `new`
- Minimise use of pointer arithmetic
- Place declarations in .h header files
- Place definitions in .cpp source files

### C++ Standard Library (STL)
- The Standard Template Library (STL) is part of the C++ standard library
- A generic (or reusable) library for managing collections of data with modern and efficient algorithms
  - Contianers: vector, list, stack
  - Algorithms, find, sort, copy

### Compiling
- The compilation process:
  - Preprocessing - handles meta-information like `#include`
  - Compiling - translates C++ into machine code objects
  - Linking - merges object into a single application

### Key C++ Concepts
#### Type
- Type is a set of values and a set of operations

1. Primitive types:
    - Built-in types: bool, char, int, float, void
    - Modifiers:
      - Signedness: signed, unsigned
      - Size: short, long, long long
2. Constructing new types using declarator operators
    - Pointer types: int*
    - References: double&
    - Array types: char[]
3. User-defined types:
    - The containers such as vector

##### Implementation-defined `<limits>`
```cpp
#include <iostream>
#include <limits>

int main() {
  std::cout << std::numeric_limits<double>::max() << "\n";
  std::cout << std::numeric_limits<double>::min() << "\n";
  std::cout << std::numeric_limits<int>::max() << "\n";
  std::cout << std::numeric_limits<int>::min() << "\n";
  // ...
}
```

##### Deciding which types to use
- Use **unsigned** for non-negative values
- Use **int** for integer arithmetic
- Use **double** for floating point computations
- Use **char** or **bool** to hold characters or truth values
- **void**: a special type with no associated values or operations. It is most often used to specify empty return values for functions

##### More (about C++)
- Mixing signed and unsigned: signed is promoted to unsigned
- Expression evaluation order
  - Order of evaluating the two operands of a binary operator is undefined (except for `&&`, `||`, and `,` (comma operator))
  ```cpp
  int i = 2;
  int j = (i = 3) * i;
  std::cout << j << std::endl; // 6 or 9
  a[i] + i++; // undefined
  foo(a[i], i++); // undefined
  f() + g(); // undefined
  ```

#### Declarations vs Definitions
- **Declarations**: makes known the type and the name of a variable
  - Introduces a name and its type into the program
  - Example
    - `int getX(Point *p);`
    - `class stack;`
    - `template <typename T> class Node;`
- **Definitions**: is a declaration, but also allocates storage for, and possibly initialises, a variable
  - A definition defines the kind of entity a name refers to
  - Example
    - `int error_no = 20;`
- A variable must be defined exactly once and must be defined or declared before it is used
- **`extern`**: declares a variable without defining it, except when it is initialised

#### Header Files
- Header files are used to store rellated declarations and help us structure source code into multiple files
- Header files allow us to make full use of C++'s support for separate compilation
- Header files
  - Should contain only logically related declarations
  - Should generally not include definitions
  - May include class definitions, some `const` objects and inline functions
  - Should always be enclosed by header guards
- **Header guards**
  ```cpp
  #ifndef MYHEADER_H
  #define MYHEADER_H
  ...
  #endif
  ```
- Can not put variable definitions and function definitions in the header file
- **One-Definition-Rule (ODR)**
  - An entity has exactly one definition in a program
  - A header file can contain:
    - class declarations and definitions
    - template declarations and definitions
    - inline function definitions
    - variable (data) declarations
  - Can not contain variable (data) and function definitions

#### Pointers
- Pointers are variables that hold the address of an object
- Allow indirect access to the objects they point to
- A pointer has a type, a name, a value, and an address of its own
- Obtain the address of an object by using `&`
- Use `*`, the dereference operator to obtain the object a pointer points to (called **pointee**)

#### `const`
- Must be initialised when defined
- The primary role of const is to specify immutability
- Many objects don't have their values changed after initialisation
  - Like many function parameters are read but not written to
- A const pointer is `* const` not `const *`
  - `char *const cp1` -> const pointer to a char
  - `char const* cp2` -> pointer to const char
  - `const char* cp3` -> pointer to const char
- **Reference**
  - C++ references are a way of aliasing objects
  - `int &r = i`
  - Must be initialised when defined and may not be rebound to a new object
  - A reference is always `const` since it must be initialised when defined and cannot be modified later
  - A reference is not itself an object unlike a pointer which is an object
  - A reference to a const object is often said to be a const reference
  - A reference to a non-const object is often said to be a non-const reference
  - Reference is safer than pointers because they can never be null

#### `auto`
- The `auto` deduction: take exactly the type on the right-hand side but strip off the top-level const and &. But the low-level const must be preserved

#### Functions
- Formal parameters are those that appear in the function definition, specifying the type and the name of each parameter
- Actual parameters are those used when calling the function
- A function must specify a return type, which maybe `void`
- A function may have an empty parameter list
  - Implicit void parameter list:
    - `int foo();`
  - Explicit void parameter list:
    - `void bar(void)`
- **Call by value**
  - The actual argument is copied into a location being used to hold the formal parameter's value during method/function execution
  - rvalue
- **Call by reference**
  - The formal parameter just acts as an alias for the actual parameter.
  - The method/function is actually using the actual parameter whenever it uses the formal parameter
  - lvalue
- **Parameter passing**
  - Passing by value is not a good idea when:
    - the function changes the value of the argument
    - the argument is a large object
    - the parameter type has no copy operation
- Return values: functions may return pointers or references but make sure these are not to local objects

##### Function Overloading
  - Family of functions, in the same scope, with the same name, but different formal parameters
  - Overload resolution: the process by which the compiler chooses the correct version of an overloaded function for execution
    1. Identify the **candidate** functions
    2. Selecting the **viable** functions
    3. Finding a **best-match**
  - If fails will produce a compile-time error

##### Inline Functions
- Inline functions are expanded "in line" at each point in the program in which it is invoked
- So the compiler must have access to the function definition in the header file in order to expand

## Week 2
### Scope
- The part of a program where a name is valid
- 5 scopes:
  1. Local (blocks/compound statements delimited by {})
  2. Function prototype (each parameter list is distinct)
  3. Function (only applies to goto labels)
  4. Namespace
  5. Class scope
- A scope starts from its point of declaration

### Namespaces
- Namespace scope: part of program for packaging names
- To avoid name clashes with multiple, independently developed libraries
- **Global scope**: the outermost namespace scope of a file
- **File scope**: the namespace scope in a file (unnamed)
  ```cpp
  namespace {
    int i = 3;
  }
  ```
- **using directive**: makes the namespace members visible as if they were declared outside the namespace at the location where the namespace definition is located
  - `using namespace std;`
- **using declaration**: introduce a name into the scope where using declaration appears
  ```cpp
  std::cout << "hello" << "\n";
  using std::cout;
  cout << "there\n";
  ```
- Namespace clashes
  ```cpp
  namespace X {
    int i, j, k;
  };

  int k;

  int main() {
    int i = 0;
    X::i; // error: i declared twice
    using X::j;
    using X::k; // hides the global k
    j++; // X::j
    k++; // X::k
  }
  ```

### I/O
- Predefined stream objects
  - `cin` - standard input
  - `cout` - standard output
  - `cerr` - standard error
- Formatting
  - `std::cout << "In hex " << std::hex << 1331 << std::endl;` - will print in hex
  - `std::cout << std::setprecision(3) << 1331.123456 << std::endl;` - print 3 d.p.
  - etc.

### Strings
- Use iterator and [] for speed
- Use `at()` when you need range checking
- Use `find()` operations to locate valules in a string
- Use `isalpha()`, `isdigit()` for character classification

### Type Casting
- **Objects**
  - An object is a region of memory with a type
  - Two kinds of objects: class object and non-class object
- **Strong Static Typing**:  type-checking happens during compilation. Compiler will produce type-mismatch errors if types are used illegally. Types must be known at compile time.
- **Type conversion**
  - `int b = true;` - a bool converted to an int
  - `int pi = 3.14;` - pi has value of 3, no compiler warning
- **Static casting**
  ```cpp
  double myDouble = 3.14;
  int cast = static_cast<int>(myDouble);
  ```
- `reinterpret_cast`: low level reinterpretation of the bit pattern
  - reat the sequence of bits (object representation) of expression as if it had the type new_type (from cppreference)
- `const_cast`: removes low level const
  - Avoid if you can
  - Used mostly for portability

### Arrays
- Use `vector` and `list` containers in STL
  ```cpp
  std::array<int, 4> a = {0, 1, 2, 3};
  ```
- Array is an indexed data structure, holds items of a single type, has a fixed size, sized at compile time, can't hold reference types, cannot be copied or assigned
- It's better to use vector than array
- Range-Based for in C++11
  ``` cpp
  std::string s("Hello world!");
  for (auto &c: s) {
    c = std::toupper(c);
  }
  ```

### STL
#### Function Templates
- A function template is a prescription for the compiler to generate a particular instances of a function varying by type
```cpp
template <typename T>
T min(T a, T b) {
  return a < b ? a : b;
}
```

#### C++ STL
- All components of the STL are templates
- Containers are copyable/movable/assignable

### Sequential Containers
#### Vector
- Most commonly used container
- Fast random access to element
- Insertion and removal of elements at the end is fast
- Insertion and removal elsewhere in the vector may be very slow
- Type must have default constructor
- [] at invalid index is undefined
- `size()` - how many elements the vector has
- `capacity()` - the number of elements the vector can hold
- `assign`
  - For different types, but type must be type-assignable

#### Other Sequential Containers
- `std::list`
  - Implemented as a doubly-linked list
  - No method for random access
  - Support for fast merging and splicing of lists
- `std::forward_list`
  - A singly-linked list with pointers from each node only to the next one
  - Takes less memory
  - No -- for iterators
- `std::deque`
  - Like vector but allows fast insertion and removal from its beginning as well
  - Using `push_front()` and `pop_front()`
- `std::array`
  - Constant-size array
  - Size must be known at compile time
  - Basically wraps a C-style array inside
- Adaptors: `stack`, `queue`, and `priority_queue`
  - Defines them as adaptors built from a basic sequence type, but with the API that we really want

### Associative Containers
- A **value type** is accessed through a second data type, the **key**
- The elements are either unordered or ordered based on their key values rather than the order of insertion
- by default, `operator<` is used to compare the keys
- `set`
  - The value type and key are the same
- `map`
  - A collection of pairs

### Iterators
- Iterator is an abstract notion of pointers
- Glue between containers and generic algorithms
- `a.begin()`: a pointer to the first element
- `a.end()`: a pointer to one past the last element
- If `p` points to the k-th element
  - `*p` is the object pointed to
  - `++p` points to (k+1)-th element
- Every iterator declared is non-const, so that it can be used to iterate through the elements in a container
- A const iterator for a container is an iterator that "points" to a const element in the container
  - This can not be used to modify the container elements
- Non const - can be used to modify the container elements
- **Reverse iterators**
  - `first`: pointer to last element
  - `last`: pointer to element before the 1st element
  - `++p` points to (k-1)-th element

#### Categories
- Input: read, access, ++, ==, !=
- Output: write, ++
- Forward: read, access, write, ++, ==, !=
- Bidirectional: read, access, write, ++, --, ==, !=
- Random: read, access (also []), write, ++, --, +, -, +=, -=, ==, !=, <, >, >=, <=

## Week 3
### Object Oriented Programming
- A class uses data abstraction and encapsulation to define an abstract data type
- **Interface**: the oprationsn used by the user
- **Implementation**: the data members, the bodies of the functions in the interface and any other function not intended for general use
- **Abstraction**: separation of interface from implementation
- **Encapsulation**: enforcement of this via information hiding
- Member access control: can be `public` or `private`
- Class scope: needs the class name and `::` if definiton outside the class header thing
- `this` pointer: member function has an extra implicit parameter, a pointer to the object on behalf of which the function is called.
- returning the object: `return *this;`
- Overloading based on `const`
  ```cpp
  const Sales_data& Sales_data::print(ostream &os) const
  Sales_data& Sales_data::print(ostream &os)
  ```
- Mutable data members: can be modified if the object is a const object
  ```cpp
  mutable unsigned num_called = 0;
  ```

### Constructors
- Define how class data members are intialised
- Has the same name as the class and no return type
- The compiler generated default constructor
  ```
  for each data member in declaration order
    if it has an in-class initialiser
      Initialise it using the in-class initialiser
    else
      if it is of a built-in type
      it is undefined // local scope
      else
        Initialise it using its default constructor
  ```

#### Constructor Initializer List
```cpp
class A {
public:
  A(int i, int j): j{i}, i{j}{}
private:
  int j;
  int i;
};
```
- The order is important! In the order variables are declared

#### Delegating Constructors
- A constructor can call another constructor
```cpp
Sales_data(const std::string &s, unsigned n, double p):
            bookNo{s}, units_sold{n}, revenue{p*n} { }

Sales_data() : Sales_data{"", 0, 0} { }

// Sales_data(const std::string &s): bookName{s} { }
// replace with:
Sales_data(const std::string &s) : Sales_data{s, 0, 0} { }

// tell this constructor to call the default constructor first
Sales_data(std::istream &is) : Sales_data{} {
  read(is, *this);
}
```

### Uniform initialisation
- "If it can possiblly be interpreted as a function prototype, then it will be"
- Solution: use {} instead of () because it can't be mistaken for function calls

### Friends
- A friend of a class can access its private members
- A class makes a function its friend by including a declaration for that function preceeded by the keyword `friend`
- 3 types of friends:
  - An ordinary function
    ```cpp
    class Sales_data {
      friend std::ostream& print(std::ostream&, const Sales_data&);
    };
    ```
  - A class
    ```cpp
    class Screen {
      friend class Window_Mgr;
    };
    ```
  - A member function of a class
    ```cpp
    class Screen2 {
      friend Window_Mgr& Window_Mgr::relocate(Screen&);
    };

### Type Conversions
- Static data member
  - Contained in the class, not in the object
  - Objects only contain non-static data members
- Implicit Type Conversions
  - Constructors can create implicit conversions from other types to the class type
  - A constructor that can be called with a single argument defines an implicit conversion
    ```cpp
    class Foo {
    public:
      Foo(const std::vector<int> v) : data{v} {}
    private:
      std::vector<int> data;
    };

    std::vector<int> ivec;
    Foo foo = ivec; // create a Foo object
    ```
- `explicit` - suppressing implicit conversion
  - `explicit Sales_data(std::istream&);`
  - To stop the implicit type conversions


### Copy Control
- **Initialisation**
  ```cpp
  MyClass one;
  MyClass two = one;
  ```
- **Assignment**
  ```cpp
  MyClass one, two;
  two = one;
  ```

Special member functions:
- Move Constructor
- Move Assignment

#### Copy Assignment
- Takes only one argument of the same type
- Returns a reference to its left-hand operand
- `A& operator=(const A &a) { ... }`

#### Copy Constructor
- A constructor is the copy ctor if its first parameter is a reference to the class type and any additional parameters have default values
  - `A (const A &a) { ... }`
  - Copy initialisation is like
    - `MyString s2 = "abc";`

#### Destructors
- Unique, can not be overloaded
- 2 things happen when called:
  - The function body of the destructor is executed
  - The members are destryoed by calling their destructors in their reverse declaration order
- This is the process of object construction in reverse
- Members of STL container types or smart pointers are automatically destroyed
- Used to free memory allocated via `new` or some other resources
- No need to provide a destructor if its data member are of non-pointer-related built-in types
- Destructors are called when the variable go out of scope

#### Preventing Copies
`NoCopy(const NoCopy &data) = delete;`
- `= delete` tells the compiler not to generate this member

## Week 4
### Copy Control
- Compiler generated copy constructor and assignment are shallow copy

#### Move Semantics
- To avoid unnecessary copies when working with temporary objects that are about to evaporate, and whole resources can safely be taken from that such a temporary object and used by another

#### General Strategies for Classes
- Value-like classes
  - Class data members have their own state
  - When an object is copied, the copy and the original are independent of each other
- Pointer-like classes
  - Class data members share state
  - When an object is copied, the copy and the original use the same underlying data
  - Changes made to the copy also affect the original, and vice versa

### `noexcept`
- Signals to the compiler and optimiser that no exception will be thrown from the method
- Automatically provided for compiler synthesised big five

### Lvalues and Rvalues
- **Lvalues**: an object's identity or address
- **Rvalues**: an object's value
- **Lvalue reference**: formed by placing `&` after some type
  ```cpp
  A a;
  A& a_ref1 = a;
  ```
  - Functions that return lvalue ref: `++i`, `*p`, `a[2]`
- **Rvalue reference**: formed by placing `&&` after some type
  ```cpp
  A a;
  A&& a_ref2 = a;
  ```
  - Functions that return rvalue ref: `i++`, `i + j`, `i < k`

### Argument Dependent Lookup
- First the normal name lookup for f is performed
- Then, look for f in the namespace scope where x is defined

### Operator Overloading
#### Rules and Exceptions
- Overload operators must have at least one operand of class or enumeration type
- `,`, `&`, `&&`, `||` should usually not be overloaded
- Usually when we make a class need to overload the subscript, call, access arrow. Also need to define some operator as friend functions (eg. `<<` and `>>`)

#### Overloading Example
- I/O Operators
  ```cpp
  std::ostream& operator<<(std::ostream &os, const Sales_data &item) {
    os << item.isbn() << " " << item.units_sold << " "
    << item.revenue << " " << item.avg_price();
  return os;
  }
  ```
- Compound Assignment Operators
  ```cpp
  Sales_data& Sales_data::operator+=(const Sales_data &rhs) {
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
  }
  ```
- Arithmetic
  - Usually non member functions
  ```cpp
  Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs) {
    Sales_data sum = lhs;
    sum += rhs;
    return sum;
  }
  ```
- Relational operators
  - Usually non member functions
  ```cpp
  Sales_data.h:
  friend bool operator== (const Sales_data&, const Sales_data&);
  friend bool operator!=(const Sales_data&, const Sales_data&);

  Sales_data.cpp:
  bool operator==(const Sales_data &lhs, const Sales_data &rhs) {
    return lhs.isbn() == rhs.isbn() &&
        lhs.units_sold == rhs.units_sold &&
        lhs.revenue == rhs.revenue;
  }

  bool operator!=(const Sales_data &lhs, const Sales_data &rhs) {
    return !(lhs == rhs);
  }
  ```
- Subscript Operator `[]`
  - setting: `int& operator[](int i);`
  - getting: `int operator[](int i) const;`
- Increment and decrement operators
  ```cpp
  // prefix
  Clock & Clock::operator++() {
    // increment
    tick();
    // return itself for assignment
    return *this;
  }

  Clock Clock::operator++(int) { // postfix
    Clock c = *this; /* Save/copy the object*/
    // Increment the object
    tick();
    // Return the original object
    return c;
  }
  ```
  - pre inc: lvalues, returns a reference
  - post inc: rvalues, return a value, it does more work
- Arrow and dereferencing operators
  - -> and * are overloaded to provide pointer-like behaviour
  - Must be member function
- Type Conversion Operators
  - Two-way user-defined conversions when defining a new class
  - Single-arg constructors: old types to the new type
  - The new type to some old types:
    - `operator type() const;`
  - Must be defined as member functions, don't take any parameters, and don't specify a return type
  - Typically conversions don't modify the object so declared `const`
  - Can also be `explicit`

### Callable Objects
#### The Call Operator()
- Must be defined as a member function
- Can be overloaded in a class to create a so-called **function object** or **functor**, an object that can act as a function
```cpp
struct AbsInt {
  int operator() (const int val) const {
    return (val < 0) ? -val : val;
  }
};
```

#### Structure of STL Algorithms
- Most STL algorithms take one of these forms:
```cpp
std::alg(beg, end, other args);
std::alg(beg, end, dest, other args);
std::alg(beg, end, beg2, other args);
std::alg(beg, end, beg2, end2, other args);
```

#### Callable Objects
- 5 kinds of callable objects
  - Functions
  - Pointers to functions
  - Objects of a class that overloads ()
  - Objects created by **bind**
  - Objects created by **lambda expressions**

### Bind objects
- A general-purpose function adaptor
- Takes a callable object and generates a new one that adapts the parameter list of the original object
  - `auto newCallable = std::bind(Callable, arg_list);`

### Lambda Functions
- Can be considered as an unnamed, inline function
```cpp
[capture list] (parameter list) -> return type {
  function body
}
```
- Capture list: a list of local variables available in the enclosing function
- Parameter list and return type can be omitted
- Can store a lambda function into a named type
- Passing arguments to a lambda
```cpp
std::stable_sort(words.begin(), words.end(),
                [](const std::string &s1, const std::string &s2) {
                  return s1.size() < s2.size();
                });
```
- Lambda captures
  - `[v1]` - capture the value
  - `[&v1]` - capture the reference
- Implicit captures
  ```cpp
  // os implicitly captured by reference
  std::for_each(wc, words.end(),
                [&, c](const string &s) {
                  os << s << c;
                });

  // c implicitly captured by value
  std::for_each(wc, words.end(),
                [=, &os](const string &s) {
                  os << s << c;
                });
  ```
  - if the first is
    - `=` - the explicitly captured variables must be captured by reference
    - `&` - the explicitly captured variables must be captured by value
- Return type
  - Must specify return type when it cannot be reduced
  - Typically deduced when there is a single return statement

## Week 5
### Exception Handling
- Exceptions: run-time anomalies
- Exception-handling: a run-time mechanism to communicate exceptions between two unrelated parts
- In C++: `throw 100;`
- `#include <stdexcept>`
- Example of exception class
  ```cpp
  class bad_push {
  public:
    bad_push(int idx) : index {idx} { }
    value() { return index; }
  private:
    int index;
  };
  ```
- Using it
  ```cpp
  void Stack::push(const int &item) {
    if (full()) throw bad_push{currentIndex};
    stack_[++top_] = item;
  }
  ```
- Try-catch block
  ```cpp
  Stack s{2};
  try {
    s.push(1);
    s.push(2);
    s.push(3);
  } catch (bad_push &e) {
    cout << "bab_push: index=" << e.value() << endl;
  } catch (bad_pop) {
    cout << "bab_pop caught!" << endl;
  }
  ```
- Catch exceptions by `const&` if possible
  - Object thrown is always copied
  - Should not catch by pointer because ownership
  - Problems with catch by value: inefficient due to object copying, causes the slicing problem, cannot exploit polymorphism and dynamic binding
- Rethrow
  - just do `throw;` inside the catch block

### Stack Unwinding
- Every object has an owner, the stack
- Like when returning from functions (go up the stack)
- Stack unwinding is the process of exiting the calling frames in the search for an exception handler
- If a new exception is thrown during stack unwinding and not caught in the function that threw it, `terminate()` is called
- In destructor, resources might not be released properly if there is exception
- All STL guarantee that their destructors are non-throwing
- RAII: Resource Acquisition is Initialisation
  - Encapsulates resources inside objects
  - Acquire resources using constructors
- Use smart pointers because they are owning

### Exception Safety
- An exception specification is part of a function's interface
  - `void push() noexcept;` - Will not throw exception
- Allows throwing exception but must be handled inside the function
- Levels of exception safety
  - **No-throw guarantee**: also known as failure transparency, operations are guaranteed to succeed and satisfy all requirements even in the presence of exceptional situations. If an exception occurs, it will be handled internally and not observed by clients
  - **Strong exception safety**: also known as commit or rollback semantics, operations can fail, but failed operations are guaranteed to have no side effects so all data retain original values
  - **Basic exception safety**: also known as no-leak guarantee, partial execution of failed operations can cause side effects, but all invariants are preserved and no resources are leaked. Any stored data will contain valid values, even if data has different values now from before the exception
  - **No exception safety**: No guarantees are made

### Lifetimes
- Lifetime: the period of time in which memory is allocated for an object
- Different kinds of objects:
  - Static objects: allocated in a global (static) area
  - Local (or automatic or stack) objects
  - Heap objects
- Named objects: (named by the programmer): their lifetimes determined by their scope
- Heap objects (unnamed objects): their lifetimes determined by the programmer

#### Local Objects
- Primitive objects are not initialised by default
- Class objects are initialised by constructors
- Memory management done by the compiler
- Every object inaccessible at its closing `}`

#### Temporary Objects
- Unnamed objects created on the stack by the compiler
- Used during reference initialization and during evaluation of the throw expression
- There are two exceptions in the destruction of full-expressions
  - The expression appears as an initializer for a declaration defining an object: the temporary object is destroyed when the initialization is complete
  - A reference is bound to a temporary object: the temporary object is destroyed at the end of the reference's lifetime

#### Static Objects
- 4 categories of objects:
  - Local static objects
  - Global objects
  - Namespace objects
  - Class static objects
- Lifetime: created when functioin is called
  ```cpp
  std::stack<int> s; // global
  namespace X {
    std::queue<short> q; // namespace
  }
  class Y {
    static std::list<char> l; // class static
  };
  ```

#### Heap (or Free-Store) Objects
- Allocated via `new` and persist until deleted via `delete`
- For array use `delete []`

### Smart Pointers
- Motivations: efficient, avoid memory leak, avoid double free and dangling pointers
- Using reference counting

### C++11 Smart Pointers
#### `unique_ptr`
- `unique_ptr` owns the object to which it points to
- The underlying object always owned by one owner only
- Copy constructor and assignment disabled
- Move used to transfer ownership
- 2 unique_ptr should not hold ownership to the same heap object

#### `shared_ptr`
- Allows multiple pointers to point to the same object
- The pointer-to object destroyed when the last pointer goes out of scoope

#### `weak_ptr`
- Points to an object managed by a shared_ptr without incrementing the reference-count

## Week 6
### Constants
- 2 notions of immutability
  - `const`: A promise not to change this value
  - `constexpr`: An indication for it to be evaluated at compile time
    - expression might be `constexpr` even if not specified
- Aggreagate classes: all its members are public, no constructors, no in-class initializer
```cpp
struct Data {
  int ival;
  string s;
};
```

### Templates Overview
- Static polymorphism: compile-time
  - Function overloading
  - Templates: polymorphical across unrelated types
  - STL containers, iterators and algorithms are templates
- Dynamic polymorphism: runtime
  - Virtual functions are polymorphic across types related by inheritance
- **Generic Programming**
  - Generalising software components so that they are independent of any particular type
- **Funtion Templates**
  - A function template is a prescription for the compiler to generate particular instances of a function varying by type
    ```cpp
    template <typename T>
    T min(T a, T b) {
      return a < b ? a : b;
    }
    ```
  - Template can take in 2 type of parameters: type and non-type

### Template Parameters
- Inside the function is just depends on T
- T is the **template type parameter**
- Placeholder for any built-in or user-defined type
- Differenet instances of the function generated depends on when the function is called

### Instantiation
- Inclusion Compilation Model
  - Templates in header files and compile only .cpp files

### Template Argument Deduction
#### Name Resolution
- 2 parties involved about a function template: designer and client
- 2-step lookup in name resolution:
  - Type-independent names: resolved at the definition of template - designer provides the declarations
  - Type-dependent names: resolved at the point of instantiation - client provides the declarations
- 3 kinds of conversions:
  - lvalue transformation
    - Def: `template <typename T> f(T* array) { }`
    - Use: ` int a[] = 1, 2; f(a); // array to pointer`
  - Qualification conversion (const and volatile)
    - Def: `template <typename T> f(const T* array) { }`
    - Use: `int a[] = 1, 2;`
     - `int *pa = &a; f(pa); // int* => const int*`
   - Conversion to a base class from a derived class
    - Def: `template <typename T> void f(Base<T> &a) { }`
    - Use
      ```cpp
      template <typename T>
      class Derived : public Base<T> { ... }
      Derived<int> d;
      f(d);
      ```

### `decltype`
#### Explicit Template Arguments
- All deducted types for the same template parameter must be the same
- Explicit template arguments: override the template argument deduction mechanism by explicitly specifying the types of the arguments to be used
```cpp
template <typename T, typename U>
auto sum(T a, U b) -> decltype(T{} + U{}) {
  return a+b;
}
```
- More on `decltype`
  ```cpp
  int i;
  decltype(i) x; // int x: i is a variable
  ```

### Specialisation
- Needed for correctness or efficiency
- min doesnt work for char* so we can do this
```cpp
typedef const char* PCC;
template<> PCC min<PCC>(PCC a, PCC b) {
  return (strcmp(a, b) < 0) ? a : b;
}
```
#### Template Default Arguments
```cpp
template <typename T, typename Pred=std::less<T>>
T minTD(T a, T b, Pred cmp = Pred()) {
  return cmp(a, b) ? a : b;
}
```

### Variadic Templates
```cpp
#include <typeinfo>

template <typename T>
void print(const T& msg) {
  std::cout << msg << " ";
}

template <typename A, typename... B>
void print(A head, B... tail) {
  print(head);
  print(tail...);
}
```

### Class Templates
- Class names cannot be overloaded in C++
- Class template does not have deduction for the functions
- The member functions have concrete type
```cpp
template <typename T>
class Stack {
public:
  void push(const T&);
  void pop();
  T& top();
private:
  std::vector<T> stack_;
};
```

### Instantiation
- Class template only instantiated once for each type
- So the same type shares static variables
- **Instantiation**: Generation of a class from a template
- **On-deman Instantiation**: a template is instantiated when an object is defined with a type that is a class tempklate
- **Lazy instantiation**: only member functions used are instantiated

#### Name Resolution
-  Same two steps as for function templates
- Type-independent names are resolved when the template is defined
- Type-dependent names are resolved when the template is instantiated

### Default Arguments
- Add `typename cont=std::deque<t>` in the template

### Friends
- 3 kinds of friend declarations:

#### Ordinary friend functions/classes
- A normal class is a friend of template class
- Declare the `friend` inside template class

#### Friend templates
- All instantiations of the template are friends
- `template <typename U> friend class Task;`
- one-to-many

#### Friend template instantiations
- one-to-one
- `friend class foobar<T>;`

## Week 7
### Member Templates
```cpp
int main() {
  std::vector<int> ivec(10)
  std::vector<int> ivec0 = ivec;
  // error: different containers
  std::list<int> ilist0 = ivec;
  // ok: ctor exists
  std::list<int> ilist(ivec.begin(), ivec.end());
  // ok: member exists
  ilist.assign(ivec.begin(), ivec.end()); ok?
}
```
- If different type for the container, the constructors do not work, so need to declare with template too
- Template function inside template class
  ```cpp
  template <typename T2, typename CONT2>
  Stack(const Stack<T2, CONT2>&);
  ```
- And then `static_cast<T>(thing)` to convert thing to type T
- Lazy instantiations: Only member functions called are instantiated
- Member templates cannot be virtual, can not build vtable for the class unless the entire program has been compiled

### Template Template Parameters (TTPs)
- Instead of providing a type, provide a template
  ```cpp
  template <typename T, template <typename U> class CONT> class Stack;
  ```

### Specialisation
- Partial Specialisation example: provides a specialised version for pointer types
  ```cpp
  template <typename T>
  class Stack<T*> {
  public:
    void push(T*);
    void T* pop();
    T* top() const;
    bool empty() const;
  private:
    std::vector<T*> stack_;
  };
  ```
- This implementation will be used for `Stack<int*> s;`
- Specialising members but not the class
- Vector of bool iterator are not dereferencable
- at vs [] - at is exception safe

### Type Traits
- A **trait** is a class or class template that characterises a type
- Get the type:
  - `using T = decltype(*begin);`
- The STL `type_traits`
  - `is_void`, `is_integral`, `is_class`, `is_pointer`, etc

#### Type Transformation
- Fix so it works with references
```cpp
template <typename T>
struct remove_reference {
  using type = T;
};

template <typename T>
struct remove_reference<T&> {
  using type = T;
};

template <typename T>
using remove_reference_t = typename remove_reference<T>::type;

template <typename U>
auto findmin(U bgin, U end) {
  using T = remove_reference_t<decltype(*begin)>;
  auto min = std::numeric_limits<T>::min();
}

```
- Check type
```cpp
template <typename T>
struct is_numeric {
  static const bool value = false;
};

// Specialisation
template <>
struct is_numeric<int> {
  static const bool value = true;
};
```
- Should not use if statement for precondition, use assert
```cpp
static_assert(is_numeric<T>::value, "Value must be numeric");
```
- Overload `operator bool()` so can treat it as bool
```cpp
template <typename T>
struct is_numeric {
  constexpr operator bool() {
    return value;
  }
  static const bool value = false;
};
```
- **Trailing return type**
```
template <typename It>
auto fcn (It beg, It end) -> decltype(*beg) {
  // loop through the range
  // return a reference to an element
  return *beg;
}
```

#### Binding
Refernce type | rvalue | const rvalue | lvalue | const lvalue
-|-|-|-|-
T&& | yes
const T&& | yes | yes
T& | | | yes
const T& | yes | yes | yes | yes
template T&& | yes | yes | yes | yes

- `const T&` binds to everythin so we use it a lot
- `T&&` binds only to non-const rvalues

##### Reference Collapsing
- `T& &` becomes `T&`
- `T& &&` becomes `T&`
- `T&& &` becomes `T&`
- `T&& &&` becomes `T&&`

#### Forwarding
- `std::forward`
```cpp
template<typename T>
T&& forward(typename remove_reference<T>::type& a) noexcept {
  return static_cast<T&&>(a);
}
```
- `forward<T>(a)` is equivalent to
  - `static_cast<[const]T&&>(a)` when a is an rvalue
  - `static_cast<[const]T&>(a)` when a is an lvalue
- Perfect Forwarding: lvalues stay as lvalues and rvalues stay as rvalues and constness is preserved

### Variadic Templates
```cpp
#include <iostream>
#include <typeinfo>

template <typename T>
void print(const T& msg) {
  std::cout << msg << " ";
}

template <typename A, typename... B>
void print(A head, B... tail) {
  print(head);
  print(tail...);
}
```

- Vector has like fill and range constructor
```cpp
class Blob {
  std::vector<std::string> v_;
public:
  template <typename... Args>
  Blob(Args&&... args) : v_(std::forward<Args>(args)...) { }
};
```

## Week 8
### Iterator Traits
- Traits define/desscribe the class properties for an iterator
```cpp
template <typename It>
class MyIt {
public:
  using difference_type = std::ptrdiff_t;
  using iterator_category = std::forward_iterator_tag;
  using value_type = T;
  using pointer = T*;
  using reference = T&;
};
```

### `enable_if` and SFINAE
- `static_assert` will fail in compile time
- Use `enable_if` if want to have functions to run under specific condition
```cpp
<template<typename T1, typename T2>
std::enable_if_t<std::is_same<T1, T2>{}, bool>
check_type(const T1& t1, const T2& t2) {
  std::cout << t1 << " and " << t2 << " are same type\n";
  return true;
}

<template<typename T1, typename T2>
std::enable_if_t<!std::is_same<T1, T2>{}, bool>
check_type(const T1& t1, const T2& t2) {
  std::cout << t1 << " and " << t2 << " are same type\n";
  return true;
}
```
- `enable_if` is an example of Substituion Failure Is Not An Error (SFINAE) which disable a function overload and backtracks to find another function overload that will compile
- If no function is found than an error will be generated

```cpp
struct EV {
  template <typename RandomIt, typename = std::enable_if_t<std::is_arithmetic<typename std::iterator_traits<RandomIt>::value_type>{}>>
  EV(RandomIt begin, RandomIt end) {
    std::cout <" hi\n";
  }
};
```

## Week 9
### Multithreading
- Common types of program workloads:
  - Sequential
  - Parallel
  - Embarassingly parallel
- Memory Models
  - Shared memory - all threads share the same memory
  - Local memory - each thread has it's own exclusive memory
  - Message Passing - each thread has it's own memory and can share data with other threads by sending them data objects through messages

#### Race condiditon
- Multiple threads with shared resources can have race condition
- `std::thread` takes in a function
  ```cpp
  std::thread t1{[&] {
    for (int j = 0; j < num; ++j) {
      i++;
    }
  }};
  ```
  - And then do `t1.join();` wait for t1 to finish
- Mutual Exclusion: Critical Section
- Part of code when accessing shared resources
- Use a Mutex to allow only one thread at a time access the critical section
- Mutex is an object shared between threads
- **Deadlock**
  - When threads can not make any progress because can not acquire resource
  - Fix by resource ordering

#### Atomic Operations
- Generally takes 1 CPU cycle
- `std::atomic` does not provide support for your own data types

#### Mutex
- in `<mutex>` header file
```cpp
std::mutex blah;
blah.lock();
// critical section
blah.unlock();
```
- Can not copy the mutex, would not compile (copy ctor is not defined)
- Possible bug: not unlock-ing after throwing exception
- Solution: use lock guard
  ```cpp
  std::lock_guard<std::mutex> guard(blah);
  ```
  - lock guard got destroyed when out of scope, the destructor unlocks the mutex
- Use `std::recursive_lock` for recursive code

#### Unique Lock
- Resource ordering when need to acquire multiple locks
  ```cpp
  bool acquired = false;
  while (!acquired) {
    lock1.lock();
    if (lock2.try_lock()) {
      acquired = true;
    } else {
      lock1.unlock();
    }
  }
  ```
- So use Unique lock
  ```cpp
  std::unique_lock<std::mutex> lock1(from.m, std::defer_lock);
  std::unique_lock<std::mutex> lock2(to.m, std::defer_lock);

  // lock both at the same time
  std::lock(lock1, lock2);
  ```

## Week 10
### Condition Variable
- Producer Consumer problem
  - Example: bounded buffer, worker consumer, web server
- Allows threads to block, release a mutex and wait until data is set by another thread or until a time period has elapsed
- Condition variables for producer consumer:
  - `hasMaterials`
  - `hasCapacity`
- Using the condition variable
```cpp
std::condition_variable hasCapacity;

// inside the producer
std::unique_lock<std::mutex> lg(materialsCountMutex);
hasCapacity.wait(lg, [this, &i] {
  if (materialsCount + i > CAPCITY)
    return false;
  return true;
});
materialsCount += i;
hasMaterials.notify_one();
```
- `std::async`
  - Function returning a `std::future`
  - `std::future` is a thing where you can check if the result is available yet
  - `std::launch::async` - to launch a new thread
  - Good to use for exception handling too
    ```cpp
     try {
       int res = future.get();
     } catch (const std::exception& ex) {
       std::cout << "something\n";
     }
    ```

### Object Oriented Programming
### Inheritance
- Ability to create new classes based on existing ones
```cpp
class Book { ... };
class Disc_book: public Book {... };
```
- `public` is the same as Java's `extends`
- `private` cannot be accessed by user code and also derived class
- `protected` - derived classes can access
- If `class Square: private Shape {...};` user can not access the code

### Memory Layout
- Up-casting: child to parent
- Down-casting: parent to child

### Polymorphism
- Allows objects of a class to be used as if they were object of another class

### Dynamic binding
- Run-time resolution of the appropriate function to invoke based on the type of the object
- static type: the type the compiler sees (compile-time)
- dynamic type: the actual type (run-time)
- If not pointer then the static and dynamic type are always the same
- `virtual`: determine at run-time what type this is, to decide which function to call
- Object is sliced when child object become a type of parent
- `static_cast` - undefined behaviour
- `dynamic_cast` - throw exception for different class (if reference)
  - If pointer, returns a `nullptr`
- Can not just copy parent to child
- Put `override` in child class member function that overrides the one in base class
  - `double area() override { ... }`
- Can also ask the compiler to provide it for you in the base class
  - `virtual double area() = 0;`
- Need to define destructor as virtual
  - `virtual ~Shape() = default;`
- Member functions
  - **Pure virtual**
    - `virtual void draw() = 0;`
    - Inherit interface only
  - **Virtual**
    - `virtual void draw();`
    - Inherit interface and optional implementation
  - **Non virtual**
    - `std::string isbn();`
    - Inherit implementation and mandatory implementation
- `final` - put this instead of `override` if the subclasses should not override

## Week 11
### Polymorphism and Dynamic Binding
- Static binding for nonvirtuals
- Dynamic binding for virtuals

### C++ Object Model
- Not represented in object: static data members, static member functions, nonvirtual functions
- Represented in an object: non static data members, virtual functions
- Each polymorphic class has a virtual table, called **vtable** containing the address for all the virtual functions
- Each object has a hidden virtual pointer **vptr**, pointing to the beginning of its vtable

#### Example vtable
```cpp
class A {                 vtable for A: [0 | &A:f(int) ]
public:                                 [1 | &A:g()    ]
  virtual void f(int);
  virtual int g();
};
class B : public A {      vtable for B: [0 | &B:f(int) ]
public:                                 [1 | &A:g()    ]
  virtual void f(int);                  [2 | &B:h()    ]
  virtual void h();
};
class C : public B {      vtable for C: [0 | &B:f(int) ]
public:                                 [1 | &A:g()    ]
  virtual int x();                      [2 | &B:h()    ]
};                                      [3 | &C:x()    ]
```

### Name Hiding
```cpp
class Fruit {
public:
  virtual void eat(float f) { std::cout << "F::e, "; }
};

class Apple : public Fruit {
public:
  virtual void eat(int i) { std::cout << "A::e, "; }
};
```
- With the code above, the `eat(int)` override the one with float
- **Overloading**: same scope, same name but different signatures
- **Overriding**: same name, same signature but different scopes (covariant return types)
- To fix it, `using`
  ```cpp
  class Apple : public Fruit {
  public:
    using Fruit::eat;
    virtual void eat(int i) { std::cout << "A::e, "; }
  }
  ```
### Covariants and Contravariants
- **Covariants**: what should return types of overriding function be?
- **Contravariants**: what should parameter types of overriding function be?

### Initialisation and Destruction
- For example if `class Book` extended by `class Disc_book` and extended again by `class Bulk_Disc_book`, then the order is
  - Book
  - Disc_book
  - Bulk_Disc_book
- And with each suboject:
  - The data members initialised in declaration order
  - The class' constructor is called
- The reverse for destructors
- Virtual destructor means the class does not get synthsised move operations
