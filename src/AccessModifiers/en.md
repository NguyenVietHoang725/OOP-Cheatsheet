# Access Modifiers in C++

In C++, **access modifiers** (also called access specifiers) control the visibility and accessibility of class members (data members and member functions) from different parts of the program. They are essential for implementing **encapsulation**, a core principle of object-oriented programming (OOP). The three primary access modifiers in C++ are `public`, `private`, and `protected`.

## Primary Access Modifiers

1. **Public**:
   - **Accessibility**: Members declared as `public` are accessible from **anywhere** in the program, including outside the class, by objects or other classes.
   - **Use Case**: Used for members that form the **interface** of the class, such as methods or data that external code needs to access directly.
   - **Example**:
     ```cpp
     class Example {
     public:
         int x; // Accessible from anywhere
         void display() { cout << x; }
     };
     int main() {
         Example obj;
         obj.x = 10; // Allowed: x is public
         obj.display(); // Allowed: display is public
         return 0;
     }
     ```

2. **Private**:
   - **Accessibility**: Members declared as `private` are accessible only **within the class** where they are defined. They cannot be accessed by objects, derived classes, or external code unless through member functions of the same class.
   - **Use Case**: Used to **hide implementation details** and protect data integrity (encapsulation).
   - **Example**:
     ```cpp
     class Example {
     private:
         int x; // Only accessible within Example
     public:
         void setX(int val) { x = val; } // Setter to modify x
         int getX() { return x; } // Getter to access x
     };
     int main() {
         Example obj;
         // obj.x = 10; // Error: x is private
         obj.setX(10); // Allowed: modifies x via public method
         cout << obj.getX(); // Outputs: 10
         return 0;
     }
     ```

3. **Protected**:
   - **Accessibility**: Members declared as `protected` are accessible **within the class** and in **derived classes** (subclasses), but not from outside the class hierarchy.
   - **Use Case**: Used when you want derived classes to access members while still protecting them from external code.
   - **Example**:
     ```cpp
     class Base {
     protected:
         int x; // Accessible in Base and derived classes
     };
     class Derived : public Base {
     public:
         void setX(int val) { x = val; } // Allowed: x is protected
     };
     int main() {
         Derived obj;
         // obj.x = 10; // Error: x is protected
         obj.setX(10); // Allowed: modifies x via derived class
         return 0;
     }
     ```

## Key Notes
- **Default Access**:
  - In a `class`, members are `private` by default.
  - In a `struct`, members are `public` by default.
- **Inheritance and Access Modifiers**:
  - The type of inheritance (`public`, `private`, `protected`) affects how base class members are accessed in derived classes. For example, `protected` members remain `protected` in a publicly inherited derived class but become inaccessible outside the class hierarchy.
- **Encapsulation**: Use `private` and `protected` to hide implementation details and ensure data integrity. Use `public` for the class’s interface.
- **Best Practices**:
  - Use `private` for data that should not be accessed directly.
  - Use `protected` for members that derived classes need to access.
  - Use `public` for the class’s interface.

## Summary
C++ has three primary access modifiers: `public`, `private`, and `protected`, which control member accessibility. Understanding these modifiers is crucial for designing robust, encapsulated, and maintainable C++ programs.