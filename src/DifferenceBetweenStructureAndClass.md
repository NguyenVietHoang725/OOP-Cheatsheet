# Difference Between Structure and Class in C++

In C++, **structures** and **classes** are used to define user-defined data types, but they differ in key aspects. Below is a concise yet comprehensive comparison:

## Comparison Table

| **Aspect**                | **Class**                                                                 | **Structure**                                                             |
|---------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **Default Access Specifier** | Members are **private** by default.                                       | Members are **public** by default.                                       |
| **Declaration Keyword**   | Declared using the **`class`** keyword.                                   | Declared using the **`struct`** keyword.                                 |
| **Primary Use**           | Used for **data abstraction**, **encapsulation**, and **inheritance**.    | Used for **grouping different data types** with minimal encapsulation.    |
| **Inheritance**           | Supports inheritance (public, private, protected).                       | Supports inheritance, typically with public inheritance.                  |
| **Access Control**        | Hides implementation details by default (private members).               | Exposes members by default (public members).                             |



## Key Examples

### 1. Default Access Specifier
- **Class**: Members are private, so direct access outside the class causes a compilation error.
```cpp
class Test {
    int x; // Private by default
};
int main() {
    Test t;
    t.x = 20; // Error: x is private
}
```

- **Structure**: Members are public, so direct access is allowed.
```cpp
struct Test {
    int x; // Public by default
};
int main() {
    Test t;
    t.x = 20; // Works fine
    cout << t.x; // Outputs: 20
}
```

### 2. Inheritance
Both classes and structures support inheritance.

```cpp
class Parent { public: int x; };
class Child : public Parent { public: int y; };
struct Base { int x; };
struct Derived : Base { int y; };
```

### 3. Use Case
- Classes: Preferred for complex systems requiring data hiding and object-oriented programming (OOP) features like inheritance and polymorphism.
- Structures: Typically used for simple data aggregation, such as grouping related variables (e.g., coordinates, records).

## Note
- In C++, structures and classes are nearly identical except for the default access specifier and their conventional use.
- Structures in C++ differ from C structures, as C++ structures can include member functions and support inheritance, unlike C.