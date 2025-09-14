# Pure Virtual Destructor in C++

In C++, a **pure virtual destructor** is a destructor declared as pure virtual in a base class, making the class abstract while ensuring proper cleanup of derived class objects in a polymorphic hierarchy. The document from GeeksforGeeks (https://www.geeksforgeeks.org/pure-virtual-destructor-c/) explains that pure virtual destructors are legal in C++ but require a function body, unlike other pure virtual functions, due to the unique way destructors are called. This article elaborates on the concept, its implementation, why a function body is necessary, and connects to related C++ topics like virtual destructors, private destructors, and copy constructors.

## What is a Pure Virtual Destructor?

- **Definition**: A pure virtual destructor is a destructor declared with the `= 0` specifier in a base class, making it pure virtual and rendering the class abstract (i.e., no objects of the class can be instantiated directly). Unlike other pure virtual functions, a pure virtual destructor must have a function body provided outside the class declaration.
- **Syntax**:
  ```cpp
  class Base {
  public:
      virtual ~Base() = 0; // Pure virtual destructor
  };
  Base::~Base() { // Function body required
      // Cleanup code
  }
  ```
- **Purpose**:
  - Ensures proper cleanup of derived class objects when deleted through a base class pointer, similar to a regular virtual destructor.
  - Makes the class abstract, preventing direct instantiation, which is useful for defining interfaces or abstract base classes.
- **Key Difference from Other Pure Virtual Functions**: While other pure virtual functions (e.g., `virtual void func() = 0`) typically do not require a body, a pure virtual destructor must have one because destructors are called in reverse order (derived to base) during object destruction.

## Why is a Function Body Required?

- **Destructor Call Order**:
  - In C++, destructors are not overridden like regular virtual functions. Instead, they are called recursively in reverse order of construction: the derived class destructor is invoked first, followed by the base class destructor.
  - If a pure virtual destructor lacks a function body, there would be no code to execute when the base class destructor is called, leading to a linker error (undefined reference).
- **Linker Enforcement**:
  - The compiler and linker require a function body for the pure virtual destructor to ensure that the base class portion of the object is properly cleaned up during destruction.
- **Abstract Class Property**:
  - Declaring a destructor as pure virtual makes the class abstract, meaning it cannot be instantiated directly. This is useful for defining interfaces where derived classes must provide their own implementations of other pure virtual functions.

## Example: Pure Virtual Destructor Without Body (Linker Error)

The document provides an example that fails due to a missing function body:

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual ~Base() = 0; // Pure virtual destructor, no body
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "~Derived() is executed\n";
    }
};

int main() {
    Base* b = new Derived();
    delete b;
    return 0;
}
```

**Output** (Linker Error):
```
undefined reference to `Base::~Base()'
error: ld returned 1 exit status
```

- **Explanation**:
  - The program attempts to delete a `Derived` object through a `Base` pointer.
  - The `Derived` destructor is called first, but the linker cannot find the body for the `Base` destructor, resulting in an error.
  - This highlights the necessity of providing a function body for a pure virtual destructor.

## Example: Pure Virtual Destructor With Body (Correct Behavior)

The document provides a corrected version:

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual ~Base() = 0; // Pure virtual destructor
};

Base::~Base() { // Function body provided
    cout << "Pure virtual destructor is called\n";
}

class Derived : public Base {
public:
    ~Derived() {
        cout << "~Derived() is executed\n";
    }
};

int main() {
    Base* b = new Derived();
    delete b;
    return 0;
}
```

**Output**:
```
~Derived() is executed
Pure virtual destructor is called
```

- **Explanation**:
  - The `Derived` object is created, and its constructor is called (implicitly, as no constructor is shown).
  - When `delete b` is called, the virtual destructor mechanism ensures the `Derived` destructor is invoked first, followed by the `Base` destructor.
  - The provided body for `Base::~Base()` is executed, ensuring proper cleanup and no linker errors.
  - The `Base` class is abstract due to the pure virtual destructor, preventing direct instantiation (`Base b;` would fail).

## Example: Abstract Class with Pure Virtual Destructor

The document illustrates that a class with a pure virtual destructor becomes abstract:

```cpp
#include <iostream>
using namespace std;

class Test {
public:
    virtual ~Test() = 0; // Pure virtual destructor
};

Test::~Test() {}

int main() {
    Test p; // Error: cannot instantiate abstract class
    Test* t1 = new Test; // Error: cannot instantiate abstract class
    return 0;
}
```

**Output** (Compilation Error):
```
[Error] cannot declare variable 'p' to be of abstract type 'Test'
[Note] because the following virtual functions are pure within 'Test':
[Note] virtual Test::~Test()
[Error] cannot allocate an object of abstract type 'Test'
[Note] since type 'Test' has pure virtual functions
```

- **Explanation**:
  - The `Test` class is abstract because it contains a pure virtual destructor.
  - Attempts to create objects (`Test p;` or `Test* t1 = new Test;`) fail because abstract classes cannot be instantiated.
  - Derived classes must provide their own destructors, and the base class destructor body ensures proper cleanup.

## Connection to Previous Topics

- **Constructors vs. Member Functions** (from previous response):
  - Like constructors, destructors are special member functions with no return type and automatic invocation. A pure virtual destructor extends this by making the class abstract while ensuring polymorphic cleanup.
- **Copy Constructor and Assignment Operator** (from previous response):
  - In classes with dynamic memory, a pure virtual destructor ensures proper cleanup during polymorphic deletion. A deep copy constructor (as discussed in shallow vs. deep copy) complements this by ensuring copied objects are independent, avoiding resource conflicts.
- **Virtual Constructor** (from previous response):
  - Constructors cannot be virtual due to the absence of a vtable during object construction. In contrast, destructors can be virtual (or pure virtual) because the vtable is available during destruction, enabling polymorphic behavior.
- **Virtual Copy Constructor** (from previous response):
  - The virtual `Clone` method relies on the copy constructor to create polymorphic copies. A pure virtual destructor in the base class ensures that these cloned objects are properly cleaned up when deleted through a base class pointer.
- **Private Destructor** (from previous response):
  - A private destructor restricts destruction to the class or its friends. It can also be pure virtual, making the class abstract while requiring a friend function or member method for deletion, as discussed earlier.
- **Virtual Destructor** (from previous response):
  - A pure virtual destructor is a special case of a virtual destructor. While a regular virtual destructor ensures proper cleanup in polymorphic hierarchies, a pure virtual destructor additionally makes the class abstract, enforcing derivation.
- **Shallow vs. Deep Copy**:
  - In classes with dynamic memory, a pure virtual destructor ensures that resources are released during polymorphic deletion. A deep copy constructor is necessary to avoid shared resource issues when copying objects, as discussed previously.

## Key Points

- **Legality in C++**:
  - Pure virtual destructors are legal and useful for defining abstract base classes that require polymorphic cleanup.
- **Function Body Requirement**:
  - Unlike other pure virtual functions, a pure virtual destructor must have a function body to avoid linker errors, as destructors are called recursively from derived to base.
- **Abstract Class**:
  - A class with a pure virtual destructor is abstract, preventing direct instantiation. Derived classes must provide their own destructors.
- **Polymorphic Cleanup**:
  - Like regular virtual destructors, pure virtual destructors ensure that derived class destructors are called when deleting through a base class pointer, avoiding undefined behavior.
- **Vtable Mechanism**:
  - The compiler uses a vtable to dispatch the correct destructor at runtime, with a hidden pointer (`vptr`) in each object pointing to the class’s vtable.

## Best Practices

- **Always Provide a Function Body**:
  - Define a body for pure virtual destructors outside the class declaration to avoid linker errors (`undefined reference`).
- **Use in Abstract Base Classes**:
  - Employ pure virtual destructors in classes intended as interfaces or abstract base classes to enforce derivation and ensure polymorphic cleanup.
- **Combine with Deep Copying**:
  - For classes with dynamic memory, implement deep copy constructors and assignment operators to complement pure virtual destructors, ensuring resource independence.
- **Handle Private Destructors Carefully**:
  - If a pure virtual destructor is private (as discussed in the private destructor response), provide friend functions or member methods for controlled deletion in polymorphic hierarchies.
- **Test Polymorphic Deletion**:
  - Verify that deleting objects through base class pointers calls the correct derived class destructors, ensuring proper cleanup in complex hierarchies.
- **Document Intent**:
  - Clearly document the use of pure virtual destructors to indicate their role in making a class abstract and ensuring polymorphic cleanup, aiding maintenance and collaboration.
- **Avoid Unnecessary Pure Virtual Destructors**:
  - Use pure virtual destructors only when the class must be abstract. If polymorphism is needed without abstraction, a regular virtual destructor suffices.

## Summary

Pure virtual destructors in C++ are a powerful feature for defining abstract base classes while ensuring proper cleanup in polymorphic hierarchies. By declaring a destructor as pure virtual with a provided body, the class becomes abstract, preventing direct instantiation, and guarantees that derived class destructors are called during deletion through a base class pointer. The requirement for a function body distinguishes pure virtual destructors from other pure virtual functions, addressing the recursive nature of destructor calls. This concept integrates with other C++ features like deep copying, virtual destructors, and private destructors, forming a critical part of robust object-oriented design in C++.