# LLVM-Report-OOPS-Principles

## 1. C++11/C++14 Features Used by LLVM

### Unnamed namespace: 

LLVM uses unnamed namespaces to prevent the access of variables,function and class by other files. Variables, functions, and classes defined in an unnamed namespace are only accessible by file in which it is defined.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ChainedIncludesSource.cpp#L29">check refrence in llvm codebase</a>

### auto keyword:

LLVM uses auto keywords at many places. This is used where the compiler knows what the function will be going to return. Mostly where the return type of function is a user defined object, an auto keyword is used. Auto keyword is also
used with a range loop.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ChainedIncludesSource.cpp#L180">check refrence1 in llvm codebase</a>
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ChainedIncludesSource.cpp#L203">check refrence2 in llvm codebase</a>


### Initializer list:

Initializer list is also used to initialize the user defined class variable inside a class whose default constructor is not present.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/AnalysisDeclContext.cpp#L548"> refrence to llvm codebase</a>

### Lambda Expressions:

Lambda Expressions are used to pass small functions as a parameter to other functions. Instead of passing function, lambda expressions are directly passed as a parameter.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/ThreadSafety.cpp#L261">refrence to llvm codebase </a>

### nullptr:

Pointers are assigned nullptr instead of null while checking some condition.
nullptr is better than null as it is convertible to all the pointers type but not normal data type.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/CFG.cpp#L1318" >refrence to llvm codebase</a>


### constexpr keyword:

LLVM source code also uses constexpr keywords with functions or variables that can be evaluated at compile time.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/CalledOnceCheck.cpp#L431">refrece to llvm codebase</a>

### override keyword:

LLVM also uses override keywords. It is used to prevent developers from making signature mistakes i.e functions that are overridden in derived class have exactly the same signature as defined in base class.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ASTConsumers.cpp#L44">refrence to llvm codebase</a>

### range loop:

range loops are also used in llvm source code where value of list of elements is required directly.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ChainedIncludesSource.cpp#L180">check refrence in llvm codebase</a>

## 2. Class Hierarchy

Most of the classes in llvm source code involve single inheritance or don't involve any inheritence. Some of the classes also involve multiple inheritance.
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ASTConsumers.cpp#L183">single level inheritence</a>
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Frontend/ASTConsumers.cpp#L31">multilevel inheritence refrence</a>

## 3. OOP Design Decisions by LLVM

LLVM source code follows encapsulation. Most of the members of class are made private and are only accessible by public functions of class. It also uses polymorphic constructors. Members' functions of class are made constant that will not modify class attributes. Many operators are also overloaded like ‘=’ in a small vector class. Many functions inherited from base class are also overridden. It also includes a mixture of virtual methods and pure virtual methods.

It also follows modularity .There is a separate class for most of the functionality.
LLVM also contains the good mixture of generic programming. Like SmallVector class is one of the examples.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/ThreadSafety.cpp#L128">const function </a>
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/llvm/include/llvm/ADT/SmallVector.h">SmallVector refrence or generic programming refrence</a>
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/llvm/include/llvm/ADT/SmallVector.h#L1226"> overloading refrence </a>
<br>
<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/Consumed.cpp#L319">Polymorphic constructors </a>

## 4. Design Pattern

LLVM uses Factory, Observer and visitor pattern. In factory pattern subclasses decide which class to instantiate. In observer pattern if one object changes state then it's dependent object also chenges automatically. In visitor pattern new operation is added without changes in the class

<a href="https://github.com/llvm/llvm-project/blob/ea069aebccd317f350be3cabdcd848476616d4da/clang/include/clang/Analysis/BodyFarm.h">Factory Pattern </a>
<a href="https://github.com/llvm/llvm-project/blob/af3a839c70adb97323fa3d122e9ab44522dca74e/clang/include/clang/AST/AttrVisitor.h"> Visitor pattern</a> 


## 5. Usage Of Iterators and their own data structures

Iterators are used in a for loop to iterate over a list of user defined abstract data types. Like there is one SmallVector class that is made with templates in c++. SmallVector is almost similar to vectors but is optimized for small size of array.
So to iterate over the SmallVectors iterators are used. Like so there are many user defined classes.

<a href="https://github.com/llvm/llvm-project/blob/main/clang/lib/Analysis/CFG.cpp#L1799">iterator refrence</a>

<a href="https://github.com/llvm/llvm-project/blob/main/llvm/include/llvm/ADT/SmallVector.h">SmallVector refrence</a>

## Other ADT:

### Immutable List:

This class represents an immutable (functional) list.
It is implemented as a smart pointer (wraps ImmutableListImpl), so it it is intended to always be copied by value as if it were a pointer.This interface matches ImmutableSet and ImmutableMap.  ImmutableList
objects should almost never be created directly, and instead shouldbe created by ImmutableListFactory objects that manage the lifetime of a group of lists.  When the factory object is reclaimed, all lists
created by that factory are released as well.

<a href="https://github.com/llvm/llvm-project/blob/main/llvm/include/llvm/ADT/ImmutableList.h">Immutable List </a>

### ArrayRef:

ArrayRef - Represent a constant reference to an array (0 or more elements consecutively in memory), i.e. a start pointer and a length.  It allows various APIs to take consecutive elements easily and conveniently.This class does not own the underlying data, it is expected to be used in situations where the data resides in some other buffer, whose lifetime extends past that of the ArrayRef. For this reason, it is not in general safe to store an ArrayRef.

<a href="https://github.com/llvm/llvm-project/blob/main/llvm/include/llvm/ADT/ArrayRef.h"> ArrayRef </a>


