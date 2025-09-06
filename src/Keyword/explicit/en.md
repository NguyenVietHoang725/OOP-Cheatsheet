# Explicit Keyword in C++

In C++, the `explicit` keyword is used to prevent constructors from performing implicit type conversions, particularly for single-argument constructors (or constructors with multiple arguments where all but one have default values). This helps avoid unintended conversions that could lead to unexpected behavior or bugs. The `explicit` keyword ensures that type conversions must be explicitly specified, improving code clarity and safety. This article explains the `explicit` keyword, its usage, and its impact on C++ programs.

## The `explicit` Keyword

- **Definition**: The `explicit` keyword is used to mark a constructor to prevent implicit type conversions, requiring explicit casting for conversions to the class type.
- **Applicability**: It is primarily used with constructors that can be called with a single argument, as these constructors (known as **conversion constructors**) allow implicit conversion from the argument type to the class type.
- **Purpose**: Prevents unintended implicit conversions that may lead to errors, ensuring that conversions are intentional and explicit.
- **Syntax**:
  ```cpp
  class ClassName {
  public:
      explicit ClassName(type param) {
          // Constructor body
      }
  };
  ```

## Usage with Examples

### 1. Implicit Conversion Without `explicit`
- **Description**: A constructor that can be called with a single argument acts as a conversion constructor, allowing implicit conversion from the argument type to the class type.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Complex {
  private:
      double real;
      double imag;
  public:
      Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) {}
      bool operator==(Complex rhs) {
          return (real == rhs.real && imag == rhs.imag);
      }
  };

  int main() {
      Complex com1(3.0, 0.0);
      if (com1 == 3.0) { // Implicit conversion: 3.0 to Complex(3.0, 0.0)
          cout << "Same";
      } else {
          cout << "Not Same";
      }
      return 0;
  }
  ```
  **Output**:
  ```
  Same
  ```
- **Explanation**: The constructor `Complex(double r = 0.0, double i = 0.0)` allows implicit conversion of `3.0` to `Complex(3.0, 0.0)`, enabling the comparison `com1 == 3.0`. This may lead to unexpected behavior if not intended.

### 2. Preventing Implicit Conversion with `explicit`
- **Description**: By marking the constructor as `explicit`, implicit conversions are disabled, and any attempt to perform such a conversion results in a compilation error.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Complex {
  private:
      double real;
      double imag;
  public:
      explicit Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) {}
      bool operator==(Complex rhs) {
          return (real == rhs.real && imag == rhs.imag);
      }
  };

  int main() {
      Complex com1(3.0, 0.0);
      // if (com1 == 3.0) { // Error: no match for operator==
      //     cout << "Same";
      // } else {
      //     cout << "Not Same";
      // }
      return 0;
  }
  ```
  **Output** (Compilation Error):
  ```
  error: no match for 'operator==' in 'com1 == 3.0e+0'
  ```
- **Explanation**: The `explicit` keyword prevents the implicit conversion of `3.0` to `Complex`, causing a compilation error when attempting `com1 == 3.0`.

### 3. Explicit Conversion with `explicit` Constructor
- **Description**: With an `explicit` constructor, conversions must be explicitly specified using a cast or direct constructor call.
- **Example**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Complex {
  private:
      double real;
      double imag;
  public:
      explicit Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) {}
      bool operator==(Complex rhs) {
          return (real == rhs.real && imag == rhs.imag);
      }
  };

  int main() {
      Complex com1(3.0, 0.0);
      if (com1 == static_cast<Complex>(3.0)) { // Explicit conversion
          cout << "Same";
      } else {
          cout << "Not Same";
      }
      return 0;
  }
  ```
  **Output**:
  ```
  Same
  ```
- **Explanation**: The explicit cast `static_cast<Complex>(3.0)` converts `3.0` to `Complex(3.0, 0.0)`, allowing the comparison. This ensures that conversions are intentional.

## Additional Notes
- **Constant Expressions**: The `explicit` specifier can be used with a constant expression. If the expression evaluates to `true`, the constructor is explicit; otherwise, it behaves as a non-explicit constructor.
  ```cpp
  class MyClass {
  public:
      explicit (std::is_same_v<int, int>) MyClass(int x) {} // Explicit constructor
  };
  ```
- **Applicability**: The `explicit` keyword is most relevant for single-argument constructors or constructors where all but one argument have default values, as these are the only constructors that can act as conversion constructors.
- **Best Practices**:
  - Use `explicit` for constructors that should not allow implicit conversions to prevent unintended type conversions.
  - Prefer explicit casts (e.g., `static_cast`) to make conversions clear and maintainable.
  - Avoid implicit conversions in critical code to improve code safety and readability.
- **Comparison with Java**: Unlike Java’s lack of an `explicit` keyword, C++ uses `explicit` to control constructor behavior explicitly, addressing issues with implicit type conversions that Java handles differently (e.g., through stricter type systems).

## Summary
The `explicit` keyword in C++ is a powerful tool for controlling type conversions by preventing implicit conversions in single-argument constructors. By requiring explicit casts, it enhances code safety, prevents unexpected behavior, and improves clarity. Understanding when and how to use `explicit` is essential for writing robust and predictable C++ programs.