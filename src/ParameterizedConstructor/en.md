# Parameterized Constructor in C++

In C++, a **parameterized constructor** is a constructor that accepts arguments to initialize an object’s data members during creation. Unlike a default constructor, which takes no arguments, a parameterized constructor allows for flexible initialization with user-provided values.

## What is a Parameterized Constructor?

A **parameterized constructor** is a special member function of a class that:
- Takes one or more parameters.
- Initializes the object’s data members using the provided arguments.
- Does not have a return type, like all constructors.

### Syntax
```cpp
class ClassName {
public:
    // Parameterized constructor
    ClassName(parameters...) {
        // Initialization logic
    }
};
```

## Key Characteristics
- **Custom Initialization**: Allows objects to be created with specific values.
- **Explicit or Implicit Call**:
  - Explicit: `ClassName obj = ClassName(arg1, arg2);`
  - Implicit: `ClassName obj(arg1, arg2);`
- **No Default Constructor**: If a parameterized constructor is defined and no default constructor is explicitly provided, the compiler will not generate a default constructor. This means creating an object without arguments (`ClassName obj;`) will cause a compilation error unless a default constructor is defined.

## Examples

1. **Basic Parameterized Constructor**:
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int x;
       A(int val) { // Parameterized constructor
           x = val;
       }
   };

   int main() {
       A a(10); // Calls parameterized constructor
       cout << a.x; // Outputs: 10
       return 0;
   }
   ```

2. **Parameterized Constructor with Multiple Parameters**:
   ```cpp
   #include <iostream>
   #include <string>
   using namespace std;

   class Student {
       int rno;
       string name;
       double fee;
   public:
       Student(int no, string n, double f) { // Parameterized constructor
           rno = no;
           name = n;
           fee = f;
       }
       void display() {
           cout << rno << "\t" << name << "\t" << fee << endl;
       }
   };

   int main() {
       Student s(1001, "Ram", 10000); // Implicit call
       s.display(); // Outputs: 1001    Ram    10000
       return 0;
   }
   ```

3. **Parameterized Constructor with Default Arguments**:
   - Default arguments allow a parameterized constructor to act like a default constructor when no arguments are provided.
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int data;
       A(int x = 5) { // Parameterized constructor with default value
           data = x;
       }
   };

   int main() {
       A a1; // Uses default value (5)
       A a2(25); // Uses provided value
       cout << a1.data << endl; // Outputs: 5
       cout << a2.data; // Outputs: 25
       return 0;
   }
   ```

4. **Using Member Initializer List**:
   - Provides a concise way to initialize data members.
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int x;
       double y;
       A(int v1, double v2) : x(v1), y(v2) {} // Member initializer list
   };

   int main() {
       A a(10, 11.5);
       cout << a.x << " " << a.y; // Outputs: 10 11.5
       return 0;
   }
   ```

## Use Cases of Parameterized Constructor
- **Custom Object Initialization**: Initialize objects with different values based on provided arguments.
- **Constructor Overloading**: Support multiple ways to create objects by defining multiple constructors with different parameter lists.
- **Flexible Design**: Enable classes to be used in various contexts by allowing specific initial states.

## Key Notes
- **Best Practice**: If a parameterized constructor is defined, explicitly define a default constructor if you need to create objects without arguments, as the compiler will not provide one automatically.
- **Initializer List**: Preferred for initializing members, especially for `const` or reference members, as it is more efficient than assigning values in the constructor body.
- **Default Arguments**: Allow a parameterized constructor to function as a default constructor, increasing flexibility.
- **Error Prevention**: Without a default constructor, attempting to create an object without arguments will cause a compilation error.