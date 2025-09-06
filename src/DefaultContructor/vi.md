# Hàm Tạo Mặc Định Trong C++

Trong C++, **hàm tạo** (constructor) là một hàm thành viên đặc biệt được gọi tự động khi một đối tượng của lớp được tạo. Trong số các loại hàm tạo khác nhau, **hàm tạo mặc định** (default constructor) là hàm tạo không nhận tham số nào. Nó đóng vai trò quan trọng trong việc khởi tạo đối tượng khi không có dữ liệu cụ thể được cung cấp. Nếu không có hàm tạo nào được định nghĩa rõ ràng trong lớp, trình biên dịch C++ sẽ tự động tạo một hàm tạo mặc định.

## Hàm Tạo Mặc Định Là Gì?

**Hàm tạo mặc định** là hàm tạo có thể được gọi mà không cần tham số. Nó có thể:
- **Được tạo tự động** bởi trình biên dịch nếu không có hàm tạo nào được định nghĩa.
- **Được định nghĩa rõ ràng** bởi lập trình viên, với hoặc không có logic khởi tạo.

### Đặc Điểm
- **Không có tham số**: Không nhận bất kỳ tham số nào.
- **Tạo bởi trình biên dịch**: Nếu không có hàm tạo nào được định nghĩa, trình biên dịch cung cấp một hàm tạo mặc định khởi tạo các thành viên với giá trị mặc định (ví dụ: 0 cho số nguyên, `nullptr` cho con trỏ).
- **Định nghĩa rõ ràng**: Lập trình viên có thể định nghĩa hàm tạo mặc định để cung cấp khởi tạo tùy chỉnh.

### Ví Dụ

1. **Không Có Hàm Tạo Do Người Dùng Định Nghĩa**:
   - Trình biên dịch tự động tạo hàm tạo mặc định.
   ```cpp
   class GFG {
   public:
       int x;
   };
   int main() {
       GFG obj; // Gọi hàm tạo mặc định do trình biên dịch tạo
       cout << obj.x; // Không xác định (chưa được khởi tạo)
       return 0;
   }
   ```

2. **Hàm Tạo Mặc Định Định Nghĩa Rõ Ràng**:
   - Lập trình viên định nghĩa hàm tạo mặc định không có logic.
   ```cpp
   class GFG {
   public:
       GFG() {} // Hàm tạo mặc định rỗng
   };
   int main() {
       GFG obj; // Gọi hàm tạo mặc định do người dùng định nghĩa
       return 0;
   }
   ```

3. **Hàm Tạo Mặc Định Với Khởi Tạo**:
   - Khởi tạo các thành viên lớp với giá trị cụ thể.
   ```cpp
   class GFG {
       int x;
   public:
       GFG() {
           x = 10;
           cout << "x được khởi tạo là " << x << endl;
       }
   };
   int main() {
       GFG obj; // Kết quả: x được khởi tạo là 10
       return 0;
   }
   ```

## Trường Hợp Sử Dụng Hàm Tạo Mặc Định
- **Tạo mảng hoặc vector của các đối tượng**: Hàm tạo mặc định cần thiết để khởi tạo các phần tử mà không có giá trị rõ ràng.
- **Thư viện STL**: Các container như `std::vector<GFG>` yêu cầu hàm tạo mặc định để tạo đối tượng.
- **Khởi tạo mặc định**: Đảm bảo các đối tượng có trạng thái ban đầu an toàn hoặc có ý nghĩa.
- **Lập trình tổng quát và template**: Hàm tạo mặc định thường được yêu cầu để tăng tính linh hoạt trong mã dựa trên template.

## Chương Trình Ví Dụ
```cpp
#include <iostream>
#include <string>
using namespace std;

class GFG {
    string name;
    int roll;
public:
    GFG() { // Hàm tạo mặc định
        name = "Unknown";
        roll = -1;
        cout << "Hàm tạo mặc định được gọi\n";
    }
    void display() {
        cout << "Tên: " << name << ", Roll: " << roll << endl;
    }
};

int main() {
    GFG s1; // Gọi hàm tạo mặc định
    s1.display(); // Kết quả: Tên: Unknown, Roll: -1
    return 0;
}
```

## Ghi Chú Quan Trọng
- **Hành Vi Trình Biên Dịch**: Nếu bất kỳ hàm tạo nào được định nghĩa (kể cả không phải hàm tạo mặc định), trình biên dịch sẽ **không** tạo hàm tạo mặc định trừ khi được khai báo rõ ràng.
- **Khởi Tạo**: Hàm tạo mặc định do trình biên dịch tạo không khởi tạo các thành viên của kiểu dữ liệu cơ bản (như `int`, `double`), có thể dẫn đến hành vi không xác định nếu truy cập.
- **Thực Tiễn Tốt Nhất**: Định nghĩa rõ ràng hàm tạo mặc định nếu cần khởi tạo cụ thể hoặc nếu các hàm tạo khác được định nghĩa để tránh phụ thuộc vào hành vi do trình biên dịch tạo.