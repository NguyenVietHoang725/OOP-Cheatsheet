# New Keyword vs Operator New in C++

In C++, memory allocation for objects is a critical aspect of dynamic memory management. The `new` keyword and `operator new` are two related but distinct mechanisms used for allocating memory on the heap. While they are often used together, they serve different purposes. This article explains the `new` keyword, `operator new`, and the differences between them, with examples to illustrate their usage.

## The `new` Keyword

- **Definition**: The `new` keyword is an operator that requests memory allocation on the heap and, for objects, invokes the constructor to initialize the allocated memory.
- **Functionality**:
  - Allocates memory using `operator new`.
  - Calls the constructor to initialize the object (if applicable).
  - Returns a pointer to the newly allocated and initialized memory.
- **Use Case**: Used to create objects dynamically, especially when the lifetime or size of the object is determined at runtime.
- **Syntax**:
  ```cpp
  Type* pointer = new Type(arguments);
  ```
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Car {
      string name;
      int num;
  public:
      Car(string a, int n) {
          cout << "Constructor called" << endl;
          name = a;
          num = n;
      }
      void display() {
          cout << "Name: " << name << endl;
          cout << "Num: " << num << endl;
      }
  };

  int main() {
      Car* p = new Car("Honda", 2017); // Allocates memory and calls constructor
      p->display();
      delete p; // Deallocates memory
      return 0;
  }
  ```
  **Output**:
  ```
  Constructor called
  Name: Honda
  Num: 2017
  ```

- **Key Points**:
  - The `new` keyword handles both memory allocation and object initialization.
  - If insufficient memory is available, it throws a `std::bad_alloc` exception (unless the nothrow version is used).
  - It is used for both built-in types (e.g., `int* p = new int;`) and user-defined types (e.g., classes).

## Operator New

- **Definition**: `operator new` is a function that allocates raw memory on the heap, similar to `malloc()` in C, but does not invoke the constructor.
- **Functionality**:
  - Allocates a block of uninitialized memory of the specified size.
  - Returns a `void*` pointer to the allocated memory.
  - Can be overloaded globally or for a specific class to customize memory allocation.
- **Use Case**: Used when custom memory allocation logic is needed, such as memory pools or debugging allocation.
- **Syntax**:
  ```cpp
  void* operator new(size_t size);
  ```
- **Example** (Overloading `operator new`):
  ```cpp
  #include <iostream>
  #include <cstdlib>
  using namespace std;

  class Car {
      string name;
      int num;
  public:
      Car(string a, int n) {
          cout << "Constructor called" << endl;
          name = a;
          num = n;
      }
      void display() {
          cout << "Name: " << name << endl;
          cout << "Num: " << num << endl;
      }
      void* operator new(size_t size) {
          cout << "new operator overloaded" << endl;
          void* p = malloc(size); // Custom allocation
          return p;
      }
      void operator delete(void* ptr) {
          cout << "delete operator overloaded" << endl;
          free(ptr); // Custom deallocation
      }
  };

  int main() {
      Car* p = new Car("HYUNDAI", 2012);
      p->display();
      delete p;
      return 0;
  }
  ```
  **Output**:
  ```
  new operator overloaded
  Constructor called
  Name: HYUNDAI
  Num: 2012
  delete operator overloaded
  ```

- **Key Points**:
  - `operator new` only allocates memory and does not call the constructor. The constructor is invoked automatically by the `new` keyword after `operator new` returns.
  - It can be overloaded globally or for a specific class to implement custom allocation strategies.
  - Overloading `operator new` requires a corresponding `operator delete` to handle deallocation consistently.

## Differences Between `new` Keyword and `operator new`

| Feature | `new` Keyword | `operator new` |
|---------|---------------|----------------|
| **Type** | Operator and keyword | Function |
| **Purpose** | Allocates memory and initializes objects (calls constructor) | Allocates raw memory only |
| **Constructor Call** | Automatically calls the constructor | Does not call the constructor |
| **Overloading** | Cannot be overloaded directly | Can be overloaded globally or per class |
| **Usage** | `Type* p = new Type(args);` | `void* p = operator new(sizeof(Type));` |
| **Return Type** | Pointer to the object type (`Type*`) | `void*` |

- **Relationship**: The `new` keyword internally calls `operator new` to allocate memory and then invokes the constructor to initialize the object. For example, `new Car("Honda", 2017)` calls `operator new(sizeof(Car))` followed by the `Car` constructor.

## Additional Notes
- **Overloading `operator new`**:
  - Overloading allows customization, such as using a memory pool or logging allocations.
  - Example of global overloading:
    ```cpp
    #include <iostream>
    #include <cstdlib>
    using namespace std;

    void* operator new(size_t size) {
        cout << "Global new operator overloaded, size: " << size << endl;
        return malloc(size);
    }

    void operator delete(void* ptr) {
        cout << "Global delete operator overloaded" << endl;
        free(ptr);
    }

    int main() {
        int* p = new int(5);
        cout << *p << endl;
        delete p;
        return 0;
    }
    ```
    **Output**:
    ```
    Global new operator overloaded, size: 4
    5
    Global delete operator overloaded
    ```

- **nothrow Version**: The `new` keyword has a `nothrow` variant (`new (std::nothrow) Type`) that returns `nullptr` instead of throwing an exception if allocation fails.
- **Best Practices**:
  - Use the `new` keyword for standard object creation and initialization.
  - Overload `operator new` only when specific memory management needs arise (e.g., custom allocators).
  - Always pair overloaded `operator new` with a corresponding `operator delete` to avoid memory leaks.
  - Be cautious with manual memory management using `operator new` alone, as it skips constructor calls and can lead to uninitialized objects.

## Summary
The `new` keyword in C++ is a high-level operator that allocates memory and initializes objects by calling their constructors, making it the standard way to create dynamic objects. In contrast, `operator new` is a lower-level function that only allocates raw memory, offering flexibility for custom memory management through overloading. Understanding their roles and differences is crucial for effective memory management in C++ programs.