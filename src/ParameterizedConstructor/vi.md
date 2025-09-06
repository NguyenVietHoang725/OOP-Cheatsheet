# Hàm Tạo Tham Số Trong C++

Trong C++, **hàm tạo tham số** (parameterized constructor) là một loại hàm tạo nhận các đối số để khởi tạo các thành viên dữ liệu của một đối tượng khi nó được tạo. Khác với hàm tạo mặc định (không nhận tham số), hàm tạo tham số cho phép khởi tạo linh hoạt với các giá trị do người dùng cung cấp.

## Hàm Tạo Tham Số Là Gì?

**Hàm tạo tham số** là một hàm thành viên đặc biệt của lớp:
- Nhận một hoặc nhiều tham số.
- Khởi tạo các thành viên dữ liệu của đối tượng bằng các đối số được cung cấp.
- Không có kiểu trả về, giống như tất cả các hàm tạo.

### Cú Pháp
```cpp
class ClassName {
public:
    // Hàm tạo tham số
    ClassName(parameters...) {
        // Logic khởi tạo
    }
};
```

## Đặc Điểm Chính
- **Khởi Tạo Tùy Chỉnh**: Cho phép tạo đối tượng với các giá trị cụ thể.
- **Gọi Rõ Ràng hoặc Ngầm Định**:
  - Rõ ràng: `ClassName obj = ClassName(arg1, arg2);`
  - Ngầm định: `ClassName obj(arg1, arg2);`
- **Không Có Hàm Tạo Mặc Định**: Nếu hàm tạo tham số được định nghĩa và không có hàm tạo mặc định được cung cấp rõ ràng, trình biên dịch sẽ không tạo hàm tạo mặc định. Điều này có nghĩa là việc tạo đối tượng mà không có đối số (`ClassName obj;`) sẽ gây lỗi biên dịch trừ khi hàm tạo mặc định được định nghĩa.

## Ví Dụ

1. **Hàm Tạo Tham Số Cơ Bản**:
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int x;
       A(int val) { // Hàm tạo tham số
           x = val;
       }
   };

   int main() {
       A a(10); // Gọi hàm tạo tham số
       cout << a.x; // Kết quả: 10
       return 0;
   }
   ```

2. **Hàm Tạo Tham Số Với Nhiều Tham Số**:
   ```cpp
   #include <iostream>
   #include <string>
   using namespace std;

   class Student {
       int rno;
       string name;
       double fee;
   public:
       Student(int no, string n, double f) { // Hàm tạo tham số
           rno = no;
           name = n;
           fee = f;
       }
       void display() {
           cout << rno << "\t" << name << "\t" << fee << endl;
       }
   };

   int main() {
       Student s(1001, "Ram", 10000); // Gọi ngầm định
       s.display(); // Kết quả: 1001    Ram    10000
       return 0;
   }
   ```

3. **Hàm Tạo Tham Số Với Tham Số Mặc Định**:
   - Tham số mặc định cho phép hàm tạo tham số hoạt động như hàm tạo mặc định khi không cung cấp đối số.
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int data;
       A(int x = 5) { // Hàm tạo tham số với giá trị mặc định
           data = x;
       }
   };

   int main() {
       A a1; // Sử dụng giá trị mặc định (5)
       A a2(25); // Sử dụng giá trị được cung cấp
       cout << a1.data << endl; // Kết quả: 5
       cout << a2.data; // Kết quả: 25
       return 0;
   }
   ```

4. **Sử Dụng Danh Sách Khởi Tạo Thành Viên**:
   - Cung cấp cách khởi tạo thành viên ngắn gọn và hiệu quả.
   ```cpp
   #include <iostream>
   using namespace std;

   class A {
   public:
       int x;
       double y;
       A(int v1, double v2) : x(v1), y(v2) {} // Danh sách khởi tạo
   };

   int main() {
       A a(10, 11.5);
       cout << a.x << " " << a.y; // Kết quả: 10 11.5
       return 0;
   }
   ```

## Trường Hợp Sử Dụng Hàm Tạo Tham Số
- **Khởi Tạo Đối Tượng Tùy Chỉnh**: Khởi tạo các đối tượng với các giá trị khác nhau dựa trên các đối số được cung cấp.
- **Nạp Chồng Hàm Tạo**: Hỗ trợ nhiều cách tạo đối tượng bằng cách định nghĩa nhiều hàm tạo với danh sách tham số khác nhau.
- **Thiết Kế Linh Hoạt**: Cho phép các lớp được sử dụng trong nhiều ngữ cảnh với các trạng thái ban đầu cụ thể.

## Ghi Chú Quan Trọng
- **Thực Tiễn Tốt Nhất**: Nếu định nghĩa hàm tạo tham số, hãy định nghĩa rõ ràng hàm tạo mặc định nếu cần tạo đối tượng mà không có đối số, vì trình biên dịch sẽ không tự động cung cấp hàm tạo mặc định.
- **Danh Sách Khởi Tạo**: Được ưu tiên để khởi tạo các thành viên, đặc biệt là với các thành viên `const` hoặc tham chiếu, vì nó hiệu quả hơn so với việc gán giá trị trong thân hàm tạo.
- **Tham Số Mặc Định**: Cho phép hàm tạo tham số hoạt động như hàm tạo mặc định, tăng tính linh hoạt.
- **Ngăn Lỗi**: Nếu không có hàm tạo mặc định, việc tạo đối tượng mà không có đối số sẽ gây lỗi biên dịch.