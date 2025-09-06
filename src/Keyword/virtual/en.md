# Virtual Keyword in C++

In C++, the `virtual` keyword is used to declare **virtual functions** in a base class, enabling **runtime polymorphism**. A virtual function allows a derived class to override the base class's implementation, ensuring that the correct function is called based on the actual object type, even when accessed through a base class pointer or reference. This article explains the `virtual` keyword, its usage, pure virtual functions, and related concepts like VTABLE and VPTR.

## Virtual Functions

- **Definition**: A virtual function is a member function declared in a base class with the `virtual` keyword, which can be overridden by a derived class to provide a specific implementation.
- **Purpose**: Ensures the correct function is called at runtime, regardless of the pointer or reference type used, achieving **runtime polymorphism**.
- **Syntax**:
  ```cpp
  class Base {
  public:
      virtual return_type functionName() {
          // Function body
      }
  };
  class Derived : public Base {
  public:
      return_type functionName() override {
          // Overridden function body
      }
  };
  ```
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void display() {
          cout << "Base class display" << endl;
      }
  };

  class Derived : public Base {
  public:
      void display() override {
          cout << "Derived class display" << endl;
      }
  };

  int main() {
      Base* basePtr;
      Derived derivedObj;
      basePtr = &derivedObj;
      basePtr->display(); // Calls Derived's display
      return 0;
  }
  ```
  **Output**:
  ```
  Derived class display
  ```

- **Key Points**:
  - Use the `override` specifier in the derived class to ensure the function correctly overrides the base class's virtual function and avoid mistakes.
  - Virtual functions are resolved at runtime using a mechanism called **late binding**.
  - If a derived class does not override a virtual function, the base class version is used.

## Pure Virtual Functions

- **Definition**: A pure virtual function is a virtual function declared in the base class without an implementation, marked with `= 0`. A class containing a pure virtual function is an **abstract class** and cannot be instantiated.
- **Syntax**:
  ```cpp
  virtual return_type functionName() = 0;
  ```
- **Purpose**: Forces derived classes to provide an implementation, ensuring a consistent interface across a class hierarchy.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void display() = 0; // Pure virtual function
  };

  class Derived : public Base {
  public:
      void display() override {
          cout << "Derived class display" << endl;
      }
  };

  int main() {
      Base* basePtr;
      Derived derivedObj;
      basePtr = &derivedObj;
      basePtr->display(); // Calls Derived's display
      return 0;
  }
  ```
  **Output**:
  ```
  Derived class display
  ```

- **Key Points**:
  - Abstract classes with pure virtual functions serve as interfaces or base classes for polymorphism.
  - Derived classes must override all pure virtual functions, or they too become abstract.

## Early Binding vs. Late Binding

- **Early Binding**: Function calls are resolved at compile time based on the type of the pointer or reference. This is used for non-virtual functions and is faster.
- **Late Binding**: Function calls are resolved at runtime based on the actual object type, using virtual functions. This is slower but enables polymorphism.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void print() {
          cout << "print base class\n";
      }
      void show() {
          cout << "show base class\n";
      }
  };

  class Derived : public Base {
  public:
      void print() override {
          cout << "print derived class\n";
      }
      void show() {
          cout << "show derived class\n";
      }
  };

  int main() {
      Base* bptr;
      Derived d;
      bptr = &d;
      bptr->print(); // Late binding: calls Derived's print
      bptr->show();  // Early binding: calls Base's show
      return 0;
  }
  ```
  **Output**:
  ```
  print derived class
  show base class
  ```

- **Explanation**: The `print()` function is virtual, so it uses late binding to call the derived class version. The `show()` function is non-virtual, so it uses early binding to call the base class version.

## How Virtual Functions Work (VTABLE and VPTR)

- **Mechanism**:
  - When a class contains a virtual function, the compiler creates a **VTABLE** (virtual table), a static array storing pointers to the virtual functions of the class.
  - Each object of the class contains a **VPTR** (virtual pointer), a hidden data member pointing to the class’s VTABLE.
  - During a virtual function call, the VPTR is used to look up the correct function address in the VTABLE at runtime.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      void fun_1() { cout << "base-1\n"; }
      virtual void fun_2() { cout << "base-2\n"; }
      virtual void fun_3() { cout << "base-3\n"; }
      virtual void fun_4() { cout << "base-4\n"; }
  };

  class Derived : public Base {
  public:
      void fun_1() { cout << "derived-1\n"; }
      void fun_2() override { cout << "derived-2\n"; }
      void fun_4(int x) { cout << "derived-4\n"; }
  };

  int main() {
      Base* p;
      Derived obj1;
      p = &obj1;
      p->fun_1();    // Early binding: base-1
      p->fun_2();    // Late binding: derived-2
      p->fun_3();    // Late binding: base-3
      p->fun_4();    // Late binding: base-4
      // p->fun_4(5); // CTE: fun_4(int) is not in Base
      return 0;
  }
  ```
  **Output**:
  ```
  base-1
  derived-2
  base-3
  base-4
  ```

- **Explanation**:
  - The compiler creates a VTABLE for `Base` with entries for `fun_2`, `fun_3`, and `fun_4`, and a VTABLE for `Derived` with updated entries for overridden functions (`fun_2`).
  - The `Derived` object’s VPTR points to the `Derived` VTABLE, enabling runtime resolution of virtual function calls.
  - Non-virtual functions (`fun_1`) use early binding, and `fun_4(int)` in `Derived` is a different function (not an override) due to its distinct signature.

## Rules for Virtual Functions
1. Virtual functions **cannot be static** because static functions are class-level and not tied to an object.
2. A virtual function can be a **friend function** of another class.
3. Virtual functions should be accessed using a **pointer or reference** of the base class type to achieve runtime polymorphism.
4. The **prototype** (return type, name, and parameters) of virtual functions must be the same in the base and derived classes.
5. Virtual functions are **defined in the base class** and optionally overridden in the derived class. If not overridden, the base class version is used.
6. A class can have a **virtual destructor** to ensure proper cleanup in polymorphic hierarchies, but it **cannot have a virtual constructor** (constructors are not polymorphic).

## Limitations of Virtual Functions
- **Performance**: Virtual function calls are slightly slower due to runtime resolution via the VTABLE, and they make compile-time optimizations harder.
- **Debugging**: In complex systems, tracking which virtual function is called can be challenging due to runtime binding.

## Best Practices
- Use the `override` specifier in derived classes to ensure correct overriding and catch errors at compile time.
- Declare destructors as virtual in base classes to ensure proper cleanup in polymorphic hierarchies.
- Use pure virtual functions to define interfaces in abstract classes.
- Minimize the use of virtual functions if performance is critical and polymorphism is not needed.

## Summary
The `virtual` keyword in C++ enables runtime polymorphism by allowing derived classes to override base class functions. Virtual functions are resolved at runtime using VTABLE and VPTR, ensuring the correct function is called based on the object’s actual type. Pure virtual functions create abstract classes, enforcing interface implementation in derived classes. Understanding virtual functions is essential for designing flexible and extensible C++ programs.