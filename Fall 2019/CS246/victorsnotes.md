# System Modelling

<!-- TOC updateonsave:false -->

- [System Modelling](#system-modelling)
  - [UML: Unified Modelling Language.](#uml-unified-modelling-language)
  - [Class Relationship 1: Composition (OWNS-A)](#class-relationship-1-composition-owns-a)
  - [Class Relationship 2: Aggregation (HAS-A)](#class-relationship-2-aggregation-has-a)
  - [Class Relationship 3: Inheritance (IS-A)](#class-relationship-3-inheritance-is-a)
    - [Constructor](#constructor)
      - [Program Example](#program-example)
    - [Protected](#protected)
    - [Method overloading](#method-overloading)
      - [Program Example](#program-example-1)
    - [Object String/coercion](#object-stringcoercion)
      - [Program Example](#program-example-2)
    - [Destructors (with inheritance)](#destructors-with-inheritance)
      - [Program Example](#program-example-3)
    - [Pure Virtual (PV) methods](#pure-virtual-pv-methods)
    - [UML](#uml)
  - [C++ Templates](#c-templates)
    - [STL: Standard Template Library](#stl-standard-template-library)
  - [Design Patterns](#design-patterns)
    - [Template Method Pattern](#template-method-pattern)
      - [NVI Idiom](#nvi-idiom)
    - [STL: map template class](#stl-map-template-class)
    - [Visitor Design Pattern](#visitor-design-pattern)
      - [Do Double dispatch](#do-double-dispatch)
      - [Adding functionality to class hierarchy without adding code](#adding-functionality-to-class-hierarchy-without-adding-code)
- [3 lectures in Download](#3-lectures-in-download)
  - [Compilation Dependencies](#compilation-dependencies)
    - [pImpl (pointer to Implementation) Idiom](#pimpl-pointer-to-implementation-idiom)
    - [Decoupling the interface (MVC Design Pattern)](#decoupling-the-interface-mvc-design-pattern)
      - [Single Responsibility Principle](#single-responsibility-principle)
  - [Exception Safety](#exception-safety)
    - [C++ Guarantee](#c-guarantee)
      - [Program Example](#program-example-4)
    - [Levels of Exception Safety](#levels-of-exception-safety)

<!-- /TOC -->

Identify the key abstractions/entities classes

Identify the relationship between classes

## UML: Unified Modelling Language.

A class in UML:

![](./AClassInUML.png)

## Class Relationship 1: Composition (OWNS-A)

```c++
class Vec {
    int x,y;
    public:
    Vec(int x, int y): x{x}, y{y} {};
};
class Basis{
    Vec v1, v2;
}

Basis b; // Will not compile, since default constructor for v1 and v2 are missing
```

Option 1: provide default constructor

Option 2: bypass the call to the default constructor, by calling a different constructor in the MIL

```c++
class Basis{
    Vec v1, v2;
    public:
    Basis(): v1{0,0}, v2{1,1} {}
}

Basis b; // Compiles
```

Embedding an object (Vec) within another object (Basis) is called composition.

`Basis OWNS-A Vec`

`A OWNS-A B` if:
- whenever A is copied, B is copied (deep);
- whenever A is destroyed, B is destroyed

`List OWNS-A Node`:

```c++
class List {
    ~~~~~
    Node *theList = ~~~~
    ~~~~~
}
```

`Node OWNS-A Node`:

```c++
struct Node {
    int data;
    Node *next;
    ~~~~~~
}
```

![](./ClassOWNSA.png)

## Class Relationship 2: Aggregation (HAS-A)

`A HAS-A B` if:
- whenever A is copied, B is copied (deep);
- whenever A is destroyed, B is not

`Basis HAS-A Vec`

![](./ClassHASA.png)

```c++
class Basis {
    Vec *vecs[100];
}
```

## Class Relationship 3: Inheritance (IS-A)

![](./ClassISA.png)

Observe `Text IS-A Book` with an extra field

### Constructor

#### Program Example

`c++/inheritance/example1`

```c++
class Book {
    string title, author;
    int numPages;
    public:
    Book(string title, string author, int numPages): title{title}, author{author}, numPages{numPages} {}
}

class Text : public Book {
    string topic;
    ~~~~~~~~~~~~~
}
```

Because a `Text` IS-A `Book`, we can do anything with a `Text` object that we could with a `Book` object. Example: both `ifstream` IS-A `istream` and `istringstream` IS-A ``. So anything you can do with an `istream` (`cin`), you can do with an `ifstream`. Subclasses inherit **all** members from the superclass.

```c++
int main() {
    Text t{}; //author is private
    t.author = "" //won't compile
}
```

Try to construct a Text:

``` c++
Text::Text(string t, string a, int n, string to): title{t}, author{a}, numPages{n}, topic{to} {}
```
Won't compile:
1. `title`, `author`, `numPages` are private in Book
1. MIL can only refer to fields declared in that class
1. Unable to default construct the superclass (Book) part of the object

Steps for object construction (3 from before)
1. space is allocated
1. superclass part is constructed
1. subclass fields are default constructed (only objects)
1. constructor body runs

Proper constructor (fixes all 3 issues)

``` c++
Text::Text(string t, string a, int n, string to): Book{t,a,n}, topic{to} {}
```

### Protected

c++ also has `protected` visibility: accessible to the class and its subclasses

```c++
class Book {
    protected:
    string author;
}

class Text : public Book {
    void addAuthor(string a) {
        author += a; // only works
    }
}

int main() {
    Text t{};
    t.author = "" // error, author is protected
}
```

Using `protected` breaks encapsulation, since subclassses can break invariants.

```c++
class Book {
    string author;
    protected:
    void addAuthor(string a) {
        // invariant check on value "a"
        author += a;
    }
}
```

### Method overloading

Book is heavy if `numPages` > 300. Text is heavy if `numPages` > 500. Comic is heavy if `numPages` > 30

#### Program Example

`c++/inheritance/example2`

```c++
class Book {
    protected:
    int numPages;
    public:
    bool isHeavy() const;
}

bool Book::isHeavy const {return numPages > 300;}
bool Text::isHeavy const {return numPages > 500;}
bool Comic::isHeavy const {return numPages > 50;}

Book b{"Title", "author"; 200};
b.isHeavy() // Book::isHeavy -> false
Comic c{"Batman", "Nomair", 40, "Nomair"};
c.isHeavy(); // Comic::isHeavy -> true
Book b = Comic{"", "", 40, ""}; // Calls book's copy constructor
b.isHeavy() // Book::isHeavy -> false
```

### Object String/coercion
```c++
Book *bp{&c};
bp->isHeavy(); // Book::isHeavy -> false
```

Compiler looks at the declared type of the variable to decide which method to call.

#### Program Example

`c++/inheritance/example3`

```c++
class Book {
    ~~~~~~~
    virtual bool isHeavy() const;
}

bool Book::isHeavy const {return numPages > 300;}
bool Text::isHeavy const override {return numPages > 500;}
bool Comic::isHeavy const override {return numPages > 50;}
```

Note: can only use `override` when rewriting a `virtual` method.

Now after using override:

```c++
Comic c{"", "", 40, ""};
Book *bp{&c};
bp->isHeavy(); // Comic::isHeavy -> true
```

Since isHeavy is vrirtual, the decision of choosing (which isHeavy is used) is delayed till the program is running. Uses the object type (runtime type). Called **DYNAMIC DISPATCH**

```
Book *collection[20];
for (int i = 0; i <> 0; i++) {
    cout << collection[i]->isHeavy();
}
```

`collection` is a polymorphic array. Polymorphism: ability to accomodate multiple types under one abstraction.

### Destructors (with inheritance)

Steps for object destruction
1. Subclass destructor runs
1. Destructor run for subclass fields that are objects
1. Superclass destructor runs
1. Space is deallocated

#### Program Example

All about destructors: `c++/inheritance/example5`

`Y` only deletes `y` not `x` since deleting `x` is `X`'s job. In main, when deleting `xp` (a pointer to `X` that's a `Y` object), it calls `X`'s destructor, not `Y`'s destructor (since destructor is not virtual) leaking the `y` array.

Advice: always make the base class's destructor virtual.

If a class with never have subclasses, declare the class "final": `class Y final : public X{};`

### Pure Virtual (PV) methods

Used for funcitons that will be implemented in subclasses and do not need to be implemented in the superclass.

```c++
class Student {
    virtual int fees() = 0; // do not need implementation
};

class Regular : public Student {
    int fees() override { ~~~~~~~~ }
};

class Coop : public Student {
    int fees() override { ~~~~~~~~ }
};
```

A subclass must implement PV methods to be considered `concrete`.

`Student s{ ~~~ }` won't compile
- class is incomplete 
- class is `abstract`

A class is `abstract` if:
1. it declares a PV method or
1. it inherits a PV method that it doesn't override

Abstract base classes are useful for:
- organizing subclasses
- declaring fields/methods common to all subclasses
- polymorphism (Student *students[36000])

### UML

- abstract -> class name italics
- public -> +
- private -> -
- protected -> #
- virtual / PV methods -> italics

## C++ Templates

```c++
template <typname T>
class Stack {
    T *contents;
    int length;
    int capacity;
    public:
    Stack();
    void push(T x);
    void pop();
    T top();
    ~Stack();
}
```

C++ Template class is parameterized on one or more types
```c++
Stack s; // won't compile: no type provided
Stack<int> s1;
s1.push(2);
Stack<char> s2;
s2.push('a');
```

```c++
template <typename T>
class List{
    struct Node {
        T data;
        Node *next;
    }
    Node *theList = nullptr;
    public:
    class Iterator {
        Node *curr;
        Iterator(Node *) ~~~~~~;
        public:
        T &operator*();
        Iterator &operator++();
        bool operator!=();
        friend class List<T>;
    }
    void addToFront(T t);
    T ith(int i);
    ~List();
}

List<int> l1;
l1.addToFront(1);
List<List<int>> l2;
l2.addToFront(l1);
```

### STL: Standard Template Library

Dynamic Length Arrays: `std::vector`.

vector - parameterized on one type, internally use a heap array.

```c++
#include <vector>

vector<int> v{3, 4}; // [3,4]

v.emplace_back(5); // [3,4,5]

for (int i = 0; i < v.size(); ++i) {}

// Iterator design pattern
for (Vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
    ~~~~~ *it ~~~~~~
}
for (auto &n: v) {
    ++n;
}

// Reverse iterator 
for (Vector<int>::reverse_iterator it = v.rbegin(); it != v.rend(); ++it) {
    ~~~~~ *it ~~~~~~
}

v.pop_back();
v.erase(v.begin()); // argument is an iterator
v.erase(v.begin() + 3); 
```

SKIP 2 LECTURES


## Design Patterns

### Template Method Pattern

The base class provides some functionality
- subclasses may/must provide parts of the behaviour.

```c++
class Turtle {
    public:
        void draw() {
            drawHead();
            drawShell();
            drawFeet();
        }
    private:
        void drawHead() {~~~~~}
        virtual void drawShell() = 0;
        void drawFeet() {~~~~~}
}

class RedTurtle : public Turtle {
    void drawShell() override { 
        // draw red shell
    }
}
```

#### NVI Idiom

NVI stands for Non-Virtual Interface

Consider a *Public* *Virtual* method

public: part of interface, come with pre/post conditions. guarantee certain behaviour

virtual: invitation to subclasses to change behaviour

This is a contradition, do not make public virtual methods.

In an NVI implementation:
- all public methods are non-virtual (except for dextructor)
- all virtual methods are private/protected

Not NVI example:

```c++
struct DigitalMedia {
    public:
        virtual void play() = 0;
        virtual ~DigitalMedia();
}
```

With NVI example:

```c++
struct DigitalMedia {
    public:
        void play() {    doPlay();    }
        virtual ~DigitalMedia();
    private:
        virtual void doPlay() = 0;
}
```

### STL: map template class
- Parameterized on two types: Key, Value
- Generalization of an array/lookup table where key should support operator<
- balanced binary tree

```c++
map<string, int> m;
m["abc"] = 5;
m["def"] = 42;
cout << m["abc"]; //prints 5
m.erase("def"); 
if(m.count("abc")) // returns 0 if not found, 1 if found

for (auto &p: m) { // Iterate in sorted key order
    cout << p.first << p.second; // p has type std::pair<string, int> with p.first is the key and p.second is the value
}

cout << m["xyz"] // when key not found, return default value
```

### Visitor Design Pattern

#### Do Double dispatch

```c++
virtual void Enemy::strike(Stick &) = 0;
virtual void Enemy::strike(Rock &) = 0;

Enemy *e = enemyFactory->createEnemy();
Wapon *w = player->chooseWeapon();
e->strike(*w); // Will not compile
```

C++ do not dynamically dispatch on parameters

```c++
class Enemy {
    virtual void strike(Weapon &w) = 0;
}

class Turtle: public Enemy {
    void strike(Weapon &w) {
        w.useOn(*this);
    }
}

class Bullet: public Enemy {
    void strike(Weapon &w) {
        w.useOn(*this);
    }
}

class Weapon {
    public:
        virtual void useOn(Turtle &) = 0;
        virtual void useOn(Bullet &) = 0;
}


Enemy *e = f->createEnemy();
Wapon *w = player->chooseWeapon();
e->strike(*w); // Will not compile
```

#### Adding functionality to class hierarchy without adding code

Program example: `lectures/se/visitor`

# 3 lectures in Download

## Compilation Dependencies

An include creates a compilation dependency. Every time an included file changes, we must recompile. Whenever possible, we should aviod including headers. Prefer forward declaring classes

```c++ 
class XYZ;
```

Benefits:

+ Reduce compilation dependencies (avoid include cycles)
+ fewer compilations
+ faster compile time

```c++
// a.h
class A {~~~~~~};

// b.h
#include "a.h"
class B : public A { // Need to know what fields/methods A has to  know what B has

}

// c.h
#include "a.h"
class C {
    A a; // Need to know size of A
}

// d.h
class A;
class D {
    A *a; // Size of A doesn't matter
}

// d.cc
#include "a.h"
void D::foo() {
    myA->boo(); // To get any of A's methods
}

// e.h
class A;
class E {
    A foo(A);
}

// e.cc
#include "a.h"
A E::foo(A a) {} // Need to know size of A to know how big of a stack frame to create for parameter/return

```

```c++
// window.h
#include <Xlib/Xlib.h>
class XWindow {
    Display *d;
    Window w;

    public:
    drawRectangle();
    drawString();
}

// client.cc
#include "window.h"
XWindow x;
x.drawRectangle();
```
Code in `client.cc` needs to recompile if window.h changes, even if the change is private.

### pImpl (pointer to Implementation) Idiom

```c++
// windowImpl.h
#include <Xlib/Xlib.h>
struct XWindowImpl { // public fields since XWindow needs access to fields
    // all private members from window.h
    Display *d;
    Window w;
}

// window.h
struct XWindowImpl
class XWindow {
    XWindowImpl *pImpl;
    public:
    drawRectangle();
    drawString();
}

// window.cc
#include "XWindowImpl"
XWindow::XWindow(): pImpl{new XWindowImpl} {}
XWindow::~XWindow() {
    delete pImpl;
}

// client.cc is same as before
```

Now if `<Xlib/Xlib.h>` changes, `client` and `window.h` doesn't need to be recompiled

Generalization: 

![](./pImpl.png)

Copuling: how modules intereact with each other. Aim: low coupling

Cohesion: how related are members within a module. Aim: high cohesion

### Decoupling the interface (MVC Design Pattern)

Model (Grid, Cell)
View (Textdisplay, graphics)
Controller (Input, who's turn, etc)

#### Single Responsibility Principle
Every class should have only one reason to change

## Exception Safety

```c++
void f() {
    MyClass *p = new MyClass;
    MyClass inc;
    g(); // g does not leak
    delete p; // ~MyClass() is correct
    // delete will not be called if g throws
}
```

Exception safety: our programs should recover from exceptions; no memory leaks, not dangling ptrs, no broken invariants.

### C++ Guarantee

During stack unwinding, all stack-allocated data is destroyed (objects' destructors run)

C++ will not automatically delete on pointers.

```c++
void f() {
    MyClass *p = new MyClass;
    try {
        MyClass inc;
        g();
        delete p;
    } catch (...) {
        delete p;
        throw;
    }
}
```

Very tedious..., maximize stack usage

RAII: Resource Acquisition Is Initialization

Wrap any resouce within a stack allocated object whose destructor releases the resource (heap memory, files)

```c++
ifstream file{"suite.txt"}; // files is opened by the constructor
// the destructor automatically closes the file

//Using 
RAII for heap memory

#include <memory>
std::unique_ptr<T> // template class
//- constructor takes a T*
//- destructor will delete the T* 
//- overloads for operator* and operator->

void f() {
    std::unique_ptr<MyClass> p{new MyClass}
    MyClass inc;
    g();
}

//To omit new:
auto p = make_unique<MyClass>();
```

#### Program Example

`lectures/c++/unique_ptr`

```c++
unique_ptr<MyClass>q = p; // leads to double free, so doesn't compile
```

Both copy constructor and copy assignment are disabled.

If ownership is shared

```c++
std::shared_ptr<T>
shared_ptr<MyClass> p = make_shared<MyClass>();
if() {
    shared_ptr<MyClass> q = p;
    // When q goes out of scope, won't dealoocate resource, up to p to do that.
}

```

Shared pointers use reference counting.

### Levels of Exception Safety

Basic Guarantee: if an exception occurs, then program is in a valid but unspecified state.

Valid: no memory leaks, no broken links

Unspecified state: don't know how much of the function was run

Strong Guarantee: if an exception occurs while executing f, it is as if f was never called.

No Throw: ...
