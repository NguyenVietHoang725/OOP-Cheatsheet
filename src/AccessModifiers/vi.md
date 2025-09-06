# Các Bộ Điều Khiển Truy Cập Trong C++

Trong C++, **bộ điều khiển truy cập** (hay còn gọi là bộ chỉ định truy cập) kiểm soát khả năng hiển thị và truy cập của các thành viên lớp (biến thành viên và hàm thành viên) từ các phần khác nhau của chương trình. Chúng là yếu tố thiết yếu để triển khai **đóng gói**, một nguyên tắc cốt lõi của lập trình hướng đối tượng (OOP). Ba bộ điều khiển truy cập chính trong C++ là `public`, `private`, và `protected`.

## Các Bộ Điều Khiển Truy Cập Chính

1. **Public**:
   - **Khả năng truy cập**: Các thành viên được khai báo là `public` có thể được truy cập từ **bất kỳ đâu** trong chương trình, bao gồm bên ngoài lớp, bởi các đối tượng hoặc lớp khác.
   - **Trường hợp sử dụng**: Dùng cho các thành viên tạo thành **giao diện** của lớp, như các phương thức hoặc dữ liệu mà mã bên ngoài cần truy cập trực tiếp.
   - **Ví dụ**:
     ```cpp
     class Example {
     public:
         int x; // Có thể truy cập từ bất kỳ đâu
         void display() { cout << x; }
     };
     int main() {
         Example obj;
         obj.x = 10; // Được phép: x là public
         obj.display(); // Được phép: display là public
         return 0;
     }
     ```

2. **Private**:
   - **Khả năng truy cập**: Các thành viên được khai báo là `private` chỉ có thể truy cập **bên trong lớp** nơi chúng được định nghĩa. Chúng không thể được truy cập bởi các đối tượng, lớp dẫn xuất, hoặc mã bên ngoài trừ khi thông qua các hàm thành viên của cùng lớp.
   - **Trường hợp sử dụng**: Dùng để **ẩn chi tiết triển khai** và bảo vệ tính toàn vẹn của dữ liệu (đóng gói).
   - **Ví dụ**:
     ```cpp
     class Example {
     private:
         int x; // Chỉ có thể truy cập trong Example
     public:
         void setX(int val) { x = val; } // Setter để sửa đổi x
         int getX() { return x; } // Getter để truy cập x
     };
     int main() {
         Example obj;
         // obj.x = 10; // Lỗi: x là private
         obj.setX(10); // Được phép: sửa đổi x qua phương thức public
         cout << obj.getX(); // Kết quả: 10
         return 0;
     }
     ```

3. **Protected**:
   - **Khả năng truy cập**: Các thành viên được khai báo là `protected` có thể truy cập **bên trong lớp** và trong **các lớp dẫn xuất** (lớp con), nhưng không thể truy cập từ bên ngoài hệ thống phân cấp lớp.
   - **Trường hợp sử dụng**: Dùng khi muốn các lớp dẫn xuất truy cập các thành viên nhưng vẫn bảo vệ chúng khỏi mã bên ngoài.
   - **Ví dụ**:
     ```cpp
     class Base {
     protected:
         int x; // Có thể truy cập trong Base và các lớp dẫn xuất
     };
     class Derived : public Base {
     public:
         void setX(int val) { x = val; } // Được phép: x là protected
     };
     int main() {
         Derived obj;
         // obj.x = 10; // Lỗi: x là protected
         obj.setX(10); // Được phép: sửa đổi x qua lớp dẫn xuất
         return 0;
     }
     ```

## Ghi Chú Quan Trọng
- **Quyền Truy Cập Mặc Định**:
  - Trong `class`, các thành viên là `private` theo mặc định.
  - Trong `struct`, các thành viên là `public` theo mặc định.
- **Kế Thừa và Bộ Điều Khiển Truy Cập**:
  - Loại kế thừa (`public`, `private`, `protected`) ảnh hưởng đến cách các thành viên lớp cơ sở được truy cập trong các lớp dẫn xuất. Ví dụ, các thành viên `protected` vẫn là `protected` trong lớp dẫn xuất kế thừa công khai nhưng không thể truy cập bên ngoài hệ thống phân cấp lớp.
- **Đóng Gói**: Sử dụng `private` và `protected` để ẩn chi tiết triển khai và đảm bảo tính toàn vẹn của dữ liệu. Sử dụng `public` cho giao diện của lớp.
- **Thực Tiễn Tốt Nhất**:
  - Sử dụng `private` cho dữ liệu không nên truy cập trực tiếp.
  - Sử dụng `protected` cho các thành viên mà các lớp dẫn xuất cần truy cập.
  - Sử dụng `public` cho giao diện của lớp.

## Tóm Tắt
C++ có ba bộ điều khiển truy cập chính: `public`, `private`, và `protected`, kiểm soát khả năng truy cập của các thành viên lớp. Hiểu rõ các bộ điều khiển này là rất quan trọng để thiết kế các chương trình C++ mạnh mẽ, được đóng gói và dễ bảo trì.