# Default Constructors in C++

In C++, a **constructor** is a special member function that is automatically invoked when an object of a class is created. Among the various types of constructors, a **default constructor** is one that takes **no arguments**. It is essential for initializing objects when no specific data is provided. If no constructor is explicitly defined in a class, the C++ compiler automatically generates a default constructor.

## What is a Default Constructor?

A **default constructor** is a constructor that can be called without any parameters. It can be:
- **Implicitly generated** by the compiler if no constructors are defined.
- **Explicitly defined** by the programmer, with or without initialization logic.

### Characteristics
- **No parameters**: Takes no arguments.
- **Compiler-generated**: If no constructors are defined, the compiler provides a default constructor that initializes members to their default values (e.g., 0 for integers, `nullptr` for pointers).
- **Explicitly defined**: Programmers can define a default constructor to provide custom initialization.

### Examples

1. **No User-Defined Constructor**:
   - The compiler generates a default constructor automatically.
   ```cpp
   class GFG {
   public:
       int x;
   };
   int main() {
       GFG obj; // Compiler-generated default constructor called
       cout << obj.x; // Undefined (uninitialized)
       return 0;
   }
   ```

2. **Explicitly Defined Default Constructor**:
   - The programmer defines a default constructor with no logic.
   ```cpp
   class GFG {
   public:
       GFG() {} // Empty default constructor
   };
   int main() {
       GFG obj; // Calls user-defined default constructor
       return 0;
   }
   ```

3. **Default Constructor with Initialization**:
   - Initializes class members to specific values.
   ```cpp
   class GFG {
       int x;
   public:
       GFG() {
           x = 10;
           cout << "x initialized to " << x << endl;
       }
   };
   int main() {
       GFG obj; // Outputs: x initialized to 10
       return 0;
   }
   ```

## Use Cases of Default Constructor
- **Creating arrays or vectors of objects**: Default constructors are required to initialize elements without explicit values.
- **STL containers**: Containers like `std::vector<GFG>` need a default constructor to create objects.
- **Default initialization**: Ensures objects have safe or meaningful initial states.
- **Generic programming and templates**: Default constructors are often required for flexibility in template-based code.

## Example Program
```cpp
#include <iostream>
#include <string>
using namespace std;

class GFG {
    string name;
    int roll;
public:
    GFG() { // Default constructor
        name = "Unknown";
        roll = -1;
        cout << "Default constructor called\n";
    }
    void display() {
        cout << "Name: " << name << ", Roll: " << roll << endl;
    }
};

int main() {
    GFG s1; // Calls default constructor
    s1.display(); // Outputs: Name: Unknown, Roll: -1
    return 0;
}
```

## Key Notes
- **Compiler Behavior**: If any constructor is defined (even a non-default one), the compiler will **not** generate a default constructor unless explicitly declared.
- **Initialization**: Compiler-generated default constructors do not initialize members of built-in types (e.g., `int`, `double`), which may lead to undefined behavior if accessed.
- **Best Practice**: Explicitly define a default constructor if you need specific initialization or if other constructors are defined to avoid relying on compiler-generated behavior.