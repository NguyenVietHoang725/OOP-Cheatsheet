# Sự Khác Nhau Giữa Struct và Class Trong C++

Trong C++, **struct** và **class** đều được sử dụng để định nghĩa các kiểu dữ liệu do người dùng xác định, nhưng chúng khác nhau ở một số khía cạnh chính. Dưới đây là so sánh ngắn gọn nhưng đầy đủ:

## Bảng So Sánh

| **Khía Cạnh**             | **Class**                                                                 | **Struct**                                                               |
|---------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **Quyền Truy Cập Mặc Định** | Các thành viên là **private** theo mặc định.                              | Các thành viên là **public** theo mặc định.                              |
| **Từ Khóa Khai Báo**      | Được khai báo bằng từ khóa **`class`**.                                   | Được khai báo bằng từ khóa **`struct`**.                                 |
| **Mục Đích Chính**        | Dùng cho **trừu tượng hóa dữ liệu**, **đóng gói**, và **kế thừa**.       | Dùng để **nhóm các kiểu dữ liệu khác nhau** với đóng gói tối thiểu.       |
| **Kế Thừa**              | Hỗ trợ kế thừa (public, private, protected).                             | Hỗ trợ kế thừa, thường sử dụng với kế thừa công khai.                    |
| **Kiểm Soát Truy Cập**   | Mặc định ẩn chi tiết triển khai (thành viên private).                   | Mặc định công khai các thành viên (public).                             |
| **Cú Pháp Ví Dụ**         | ```cpp<br>class ClassName {<br>private:<br>    int x;<br>public:<br>    void func();<br>};<br>``` | ```cpp<br>struct StructName {<br>    int x;<br>    void func();<br>};<br>``` |

## Ví Dụ Minh Họa

### 1. Quyền Truy Cập Mặc Định
- **Class**: Các thành viên là private, nên truy cập trực tiếp từ bên ngoài gây lỗi biên dịch.
  ```cpp
  class Test {
      int x; // Private theo mặc định
  };
  int main() {
      Test t;
      t.x = 20; // Lỗi: x là private
  }
  ```
- **Struct**: Các thành viên là public, nên cho phép truy cập trực tiếp.
  ```cpp
  struct Test {
      int x; // Public theo mặc định
  };
  int main() {
      Test t;
      t.x = 20; // Hoạt động tốt
      cout << t.x; // Kết quả: 20
  }
  ```

### 2. Kế Thừa
- Cả class và struct đều hỗ trợ kế thừa.
  ```cpp
  class Parent { public: int x; };
  class Child : public Parent { public: int y; };
  struct Base { int x; };
  struct Derived : Base { int y; };
  ```

### 3. Trường Hợp Sử Dụng
- **Class**: Thích hợp cho các hệ thống phức tạp yêu cầu ẩn dữ liệu và các tính năng lập trình hướng đối tượng (OOP) như kế thừa và đa hình.
- **Struct**: Thường dùng để nhóm dữ liệu đơn giản, như các biến liên quan (ví dụ: tọa độ, bản ghi).

## Ghi Chú
- Trong C++, struct và class gần như giống nhau, ngoại trừ quyền truy cập mặc định và cách sử dụng theo quy ước.
- Struct trong C++ khác với struct trong C, vì struct trong C++ có thể chứa hàm thành viên và hỗ trợ kế thừa, không giống như trong C.