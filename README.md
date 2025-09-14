# OOP Cheatsheet

## Overview

| SNo | Topic | Link | English Summary | Vietnamese Summary |
|-----|-------|------|-----------------|--------------------|
| 1 | What is OOP ? | [view](https://www.w3schools.com/cpp/cpp_oop.asp) | OOP is a programming paradigm based on objects that contain data (fields) and methods (functions). It supports principles like encapsulation, inheritance, polymorphism, and abstraction. | Lập trình hướng đối tượng (OOP) là một mô hình lập trình dựa trên **đối tượng**, trong đó đối tượng chứa dữ liệu (thuộc tính) và các phương thức (hàm). OOP hỗ trợ các nguyên tắc như đóng gói, kế thừa, đa hình và trừu tượng hóa. |
| 2 | How OOP is related to real world ? | [view](https://medium.com/@kkajasu/object-oriented-programming-with-real-world-example-63f69f917d62) | OOP models real-world entities as objects. For example, a "Car" class can have attributes (color, model) and methods (drive, stop). This makes programming closer to real-world thinking. | OOP mô phỏng các thực thể trong thế giới thực thành các **đối tượng**. Ví dụ: lớp “Car” có thuộc tính (màu, mẫu xe) và phương thức (chạy, dừng). Điều này giúp lập trình gần gũi hơn với tư duy thực tế. |
| 3 | Why to study OOPs ? | [view](https://www.c-sharpcorner.com/forums/why-we-need-oops-in-programming-language) | OOP improves code reusability, modularity, maintainability, and scalability. It makes software easier to manage and extend. | Học OOP giúp cải thiện **tái sử dụng code**, tính mô-đun, dễ bảo trì và mở rộng. Từ đó, phần mềm dễ quản lý và nâng cấp hơn. |
| 4 | Limitations of OOPs | [view](https://resources.saylor.org/wwwresources/archived/site/wp-content/uploads/2013/02/CS101-2.1.2-AdvantagesDisadvantagesOfOOP-FINAL.pdf) | OOP can be more complex, requires more resources, and may not be efficient for small programs. It can also lead to performance overhead due to abstraction. | OOP có thể phức tạp, tốn tài nguyên và không hiệu quả cho chương trình nhỏ. Ngoài ra còn có thể gây **giảm hiệu năng** do lớp trừu tượng. |
| 5 | When we say that "X" language is object oriented programming language, then what does we mean by that ? | homework | A language is considered OOP if it supports key features: encapsulation, inheritance, polymorphism, and abstraction. | Một ngôn ngữ được coi là hướng đối tượng nếu hỗ trợ các tính năng chính: **đóng gói, kế thừa, đa hình, trừu tượng hóa**. |

## Class

| SNo | Topic | Link | English Summary | Vietnamese Summary |
|-----|-------|------|-----------------|--------------------|
| 1 | Difference between Structure and Class | [view](https://www.geeksforgeeks.org/structure-vs-class-in-cpp/) | [eng verion](/src/DifferenceBetweenStructureAndClass/en.md) | [vie version](/src/DifferenceBetweenStructureAndClass/vi.md) |
| 2 | When to use Structure over Class | [view](https://stackoverflow.com/questions/54585/when-should-you-use-a-class-vs-a-struct-in-c) | [eng version](/src/WhenToUseAStructureOverAClass/en.md) | [vie version](/src/WhenToUseAStructureOverAClass/vi.md) |
| 3 | Access Modifiers | [view](https://www.w3schools.com/cpp/cpp_access_specifiers.asp) | [eng version](/src/AccessModifiers/en.md) | [vie version](/src/AccessModifiers/vi.md) |
| 4 | Friend Keyword | [view](https://www.geeksforgeeks.org/cpp/friend-class-function-cpp/) | [eng version](/src/FriendKeyword/en.md) | [vie version](/src/FriendKeyword/vi.md) |
| 5 | Member Function | [view](https://www.studytonight.com/cpp/types-of-member-function.php) | [eng version](/src/MemberFunction/en.md) | [vie version](/src/MemberFunction/vi.md) |
| 6 | Operator Overloading | [view](https://www.geeksforgeeks.org/cpp/operator-overloading-cpp/) | [eng version](/src/OperatorOverloading/en.md) | [vie version](/src/OperatorOverloading/vi.md) - [bonus](/src/OperatorOverloading/bonus.md) |
| 7 | [Constructor](#constructor) | [view](https://www.geeksforgeeks.org/constructors-c/) | A constructor is a special member function of a class that is automatically called when an object is created. It initializes the object’s data members. | Hàm tạo (constructor) là một hàm thành viên đặc biệt của lớp, được tự động gọi khi đối tượng được khởi tạo. Nó dùng để khởi gán các thuộc tính dữ liệu của đối tượng. |
| 8 | [Destructor](#destructor) | [view](https://www.geeksforgeeks.org/destructors-c/) | A destructor is a special member function of a class that is automatically called when an object is destroyed. It is mainly used to release resources. | Hàm hủy (destructor) là một hàm thành viên đặc biệt của lớp, được tự động gọi khi đối tượng bị hủy. Nó chủ yếu dùng để giải phóng tài nguyên. |

## Constructor

| SNo | Topic | Link | English Summary | Vietnamese Summary |
|-----|-------|------|-----------------|-------------------|
| 1 | Default Constructor | [view](https://www.geeksforgeeks.org/cpp/default-constructors-in-cpp/) | [eng version](/src/DefaultContructor/en.md) | [vie version](/src/DefaultContructor/vi.md) |
| 2 | Parameterised Constructor | [view](https://www.geeksforgeeks.org/cpp/parameterized-constructor-in-cpp/) | [eng version](/src/ParameterizedConstructor/en.md) | [vie version](/src/ParameterizedConstructor/vi.md) |
| 3 | Copy Constructor: Copy Constructor vs Assignment + Deep vs Shallow Copy| [view](https://www.geeksforgeeks.org/copy-constructor-vs-assignment-operator-in-c/) </br> [view](https://www.geeksforgeeks.org/difference-between-shallow-and-deep-copy-of-a-class/) | [eng version](/src/CopyConstructor/en.md) | [vie version](/src/CopyConstructor/vi.md) |
| 4 | Virtual Constructor | [view](https://www.tutorialspoint.com/virtual-constructor-in-cplusplus) | [eng version](/src/VirtualConstructor/en.md) | [vie version](/src/VirtualConstructor/vi.md) |
| 5 | Virtual Copy Constructor | [view](https://www.tutorialspoint.com/virtual-copy-constructor-in-cplusplus) | [eng version](/src/VirtualCopyConstructor/en.md) | [vie version](/src/VirtualCopyConstructor/vi.md) |

## Destructor

| SNo | Topic | Link | English Summary | Vietnamese Summary |
|-----|-------|------|-----------------|-------------------|
| 1 | Private Destructor | [view](https://www.geeksforgeeks.org/cpp/private-destructor-in-cpp/) | [eng version](/src/PrivateDestructor/en.md) | [vie version](/src/PrivateDestructor/vi.md) |
| 2 | Virtual Destructor | [view](https://www.geeksforgeeks.org/cpp/virtual-destructor/) | [eng version](/src/VirtualDestructor/en.md) | [vie version](/src/VirtualDestructor/vi.md) |
| 3 | Pure Virtual Destructor | [view](https://www.geeksforgeeks.org/cpp/pure-virtual-destructor-c/) | [eng version](/src/PureVirtualDestructor/en.md) | [vie version](/src/PureVirtualDestructor/vi.md) |
| 3 | Pure Virtual Destructor | [view](https://www.geeksforgeeks.org/cpp/pure-virtual-destructor-c/) | [eng version](/src/PureVirtualDestructor/en.md) | [vie version](/src/PureVirtualDestructor/vi.md) |
| 4 | Tips & Notes on Destructor | [view](https://stackoverflow.com/questions/461203/when-to-use-virtual-destructors) </br> [view](https://stackoverflow.com/questions/21934810/why-a-class-has-only-one-destructor/21934951) </br> [view](https://stackoverflow.com/questions/22491174/when-do-we-need-to-define-destructors) | [eng version](/src/Tip&NoteOnDestructor/en.md) | [vie version](/src/Tip&NoteOnDestructor/vi.md) |

## Object

## Important Keywords

SNo | Topic | Link | English Summary | Vietnamese Summary |
----|-------|------|----------------|--------------------|
1 | static | [view](https://www.geeksforgeeks.org/cpp/static-keyword-cpp/) | [eng version](/src/Keyword/static/en.md) | [vie version](/src/Keyword/static/vi.md) |
2 | virtual | [view](https://www.geeksforgeeks.org/cpp/virtual-function-cpp/) | [eng version](/src/Keyword/virtual/en.md) | [vie version](/src/Keyword/virtual/vi.md) |
3 | final | [view](https://www.geeksforgeeks.org/cpp/c-final-specifier/) | [eng version](/src/Keyword/final/en.md) | [vie version](/src/Keyword/final/vi.md) |
4 | explicit | [view](https://www.geeksforgeeks.org/cpp/use-of-explicit-keyword-in-cpp/) | [eng version](/src/Keyword/explicit/en.md) | [vie version](/src/Keyword/explicit/vi.md) |
5 | this | [view](https://www.javatpoint.com/this-keyword) | [eng version](/src/Keyword/this/en.md) | [vie version](/src/Keyword/this/vn.md) |
6 | new | [view](https://www.geeksforgeeks.org/cpp/new-vs-operator-new-in-cpp/) | [eng version](/src/Keyword/new/en.md) | [view version](/src/Keyword/new/vi.md) |
7 | const | [view](https://www.geeksforgeeks.org/cpp/const-keyword-in-cpp/) | [eng version](/src/Keyword/const/en.md) | [vie version](/src/Keyword/const/vi.md) |

## Features of OOPs:
- Polymorphism
- Inheritance
- Encapsulation
- Abstraction

