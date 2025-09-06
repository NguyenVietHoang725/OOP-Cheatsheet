# Const Keyword in C++

In C++, the `const` keyword is used to declare variables, pointers, member functions, function parameters, and return types as constant, preventing their modification after initialization. This promotes immutability, enhances code safety, and supports encapsulation in object-oriented programming. This article explains the various uses of the `const` keyword with examples.

## Uses of the `const` Keyword

### 1. Constant Variables
- **Definition**: A `const` variable cannot be modified after initialization and must be initialized at the time of declaration.
- **Rules**:
  - Must be initialized during declaration.
  - Cannot be assigned a new value after initialization.
  - Any attempt to modify a `const` variable results in a compile-time error (CTE).
- **Syntax**:
  ```cpp
  const data_type variable_name = value;
  ```
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // const int x; // CTE: uninitialized const
      // x = 9; // CTE: cannot assign to const
      const int y = 10;
      // y = 20; // CTE: cannot modify const
      cout << y; // Outputs: 10
      return 0;
  }
  ```

### 2. Const with Pointer Variables
The `const` keyword can be used with pointers in three distinct ways, affecting either the pointer, the data it points to, or both.

#### a. Pointer to a Const Value
- **Syntax**: `const data_type* var_name;`
- **Description**: The data pointed to is constant and cannot be modified, but the pointer itself can point to a different address.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 10;
      char y = 'M';
      const int* i = &x;
      const char* j = &y;
      x = 9; // Allowed: x is not const
      y = 'A'; // Allowed: y is not const
      // *i = 6; // CTE: cannot modify const int
      // *j = 'B'; // CTE: cannot modify const char
      cout << *i << " " << *j; // Outputs: 9 A
      return 0;
  }
  ```

#### b. Const Pointer to a Non-Const Value
- **Syntax**: `data_type* const var_name;`
- **Description**: The pointer is constant and cannot point to a different address, but the data it points to can be modified.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 5, z = 6;
      char y = 'A', p = 'C';
      int* const i = &x;
      char* const j = &y;
      *i = 10; // Allowed: data can be modified
      *j = 'D'; // Allowed: data can be modified
      // i = &z; // CTE: cannot change const pointer
      // j = &p; // CTE: cannot change const pointer
      cout << *i << " and " << *j << endl; // Outputs: 10 and D
      return 0;
  }
  ```

#### c. Const Pointer to a Const Value
- **Syntax**: `const data_type* const var_name;`
- **Description**: Both the pointer and the data it points to are constant, preventing any modification.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 9;
      const int* const i = &x;
      // *i = 10; // CTE: cannot modify const data
      // i = nullptr; // CTE: cannot change const pointer
      char y = 'A';
      const char* const j = &y;
      // *j = 'B'; // CTE: cannot modify const data
      // j = nullptr; // CTE: cannot change const pointer
      cout << *i << " and " << *j; // Outputs: 9 and A
      return 0;
  }
  ```

#### d. Passing Pointers to Functions
- Passing a `const` pointer to a function expecting a non-const pointer parameter causes a CTE, as it violates the const-correctness.
- **Example** (Error Case):
  ```cpp
  #include <iostream>
  using namespace std;

  int foo(int* y) { return *y; }

  int main() {
      int z = 8;
      const int* x = &z;
      // cout << foo(x); // CTE: cannot convert const int* to int*
      return 0;
  }
  ```
- **Example** (Correct Case):
  ```cpp
  #include <iostream>
  using namespace std;

  void printfunc(int* ptr) {
      cout << "Value: " << *ptr << endl;
      cout << "Address of ptr: " << &ptr << endl;
  }

  int main() {
      int x = 10;
      int* const i = &x;
      printfunc(i); // Allowed: new pointer created
      cout << "Address of i: " << &i << endl;
      return 0;
  }
  ```
  **Output**:
  ```
  Value: 10
  Address of ptr: [some address]
  Address of i: [different address]
  ```

- **Summary of Pointers**:
  - `const int* ptr`: Pointer to a const value (data is immutable).
  - `int* const ptr`: Const pointer to a non-const value (pointer is immutable).
  - `const int* const ptr`: Const pointer to a const value (both are immutable).

### 3. Constant Member Functions
- **Definition**: A member function declared with `const` ensures it does not modify the object’s state (except for `mutable` members).
- **Syntax**:
  ```cpp
  return_type function_name() const;
  ```
- **Description**:
  - Can be called on both const and non-const objects.
  - Ensures the function does not modify non-mutable members.
  - Const objects can only call const member functions.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Test {
      int value;
  public:
      Test(int v = 0) : value(v) {}
      int getValue() const { // Const member function
          // value = 100; // CTE: cannot modify object
          return value;
      }
      void setValue(int val) { // Non-const member function
          value = val;
      }
  };

  int main() {
      Test t(20);
      cout << t.getValue() << endl; // Outputs: 20
      const Test t_const(10);
      cout << t_const.getValue() << endl; // Outputs: 10
      // t_const.setValue(15); // CTE: const object cannot call non-const function
      t.setValue(12);
      cout << t.getValue() << endl; // Outputs: 12
      return 0;
  }
  ```

### 4. Constant Function Parameters and Return Types
- **Constant Parameters**:
  - Declaring a parameter as `const` prevents modification within the function.
  - Passing a const value to a non-const parameter causes a CTE when passed by reference.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  void foo(const int y) {
      // y = 6; // CTE: cannot modify const parameter
      cout << y;
  }

  void foo1(int y) {
      y = 5;
      cout << y;
  }

  int main() {
      int x = 9;
      const int z = 10;
      foo(z); // Outputs: 10
      cout << endl;
      foo1(x); // Outputs: 5
      return 0;
  }
  ```

- **Constant Return Type**:
  - A `const` return type ensures the returned value cannot be modified.
  - **Example**:
    ```cpp
    #include <iostream>
    using namespace std;

    const int foo(int y) {
        y--;
        return y;
    }

    int main() {
        int x = 9;
        const int z = 10;
        cout << foo(x) << '\n' << foo(z); // Outputs: 8 9
        return 0;
    }
    ```

- **Const Parameter and Const Return Type**:
  - Both parameter and return type are const, preventing modification.
  - **Example**:
    ```cpp
    #include <iostream>
    using namespace std;

    const int foo(const int y) {
        // y = 9; // CTE: cannot modify const parameter
        return y;
    }

    int main() {
        int x = 9;
        const int z = 10;
        cout << foo(x) << '\n' << foo(z); // Outputs: 9 10
        return 0;
    }
    ```

## Key Notes
- **Initialization**: Const variables and objects must be initialized at declaration, typically using constructors for objects.
- **Const-Correctness**: Ensures immutability, reducing errors and improving code clarity.
- **Error Cases**:
  - Modifying a const variable or object results in a CTE.
  - Passing a const value to a non-const parameter by reference causes a CTE.
  - Non-const member functions cannot be called on const objects.
- **Best Practices**:
  - Use `const` for variables, pointers, and parameters that should not change.
  - Declare member functions as `const` when they do not modify the object.
  - Use `const` return types to prevent unintended modifications of returned values.

## Summary
The `const` keyword in C++ is a powerful tool for enforcing immutability across variables, pointers, member functions, parameters, and return types. By preventing unintended modifications, it enhances code safety and supports encapsulation. Understanding its various applications is essential for writing robust and maintainable C++ programs.