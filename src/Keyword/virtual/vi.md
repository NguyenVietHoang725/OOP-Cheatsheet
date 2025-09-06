# Từ Khóa `virtual` Trong C++

Trong C++, từ khóa `virtual` được sử dụng để khai báo **hàm ảo** trong lớp cơ sở, hỗ trợ **đa hình thời gian chạy** (runtime polymorphism). Hàm ảo cho phép lớp dẫn xuất ghi đè triển khai của lớp cơ sở, đảm bảo rằng hàm đúng được gọi dựa trên loại đối tượng thực tế, ngay cả khi được truy cập qua con trỏ hoặc tham chiếu của lớp cơ sở. Bài viết này giải thích từ khóa `virtual`, cách sử dụng, hàm ảo thuần túy, và các khái niệm liên quan như VTABLE và VPTR.

## Hàm Ảo

- **Định nghĩa**: Hàm ảo là hàm thành viên được khai báo trong lớp cơ sở với từ khóa `virtual`, có thể được ghi đè bởi lớp dẫn xuất để cung cấp triển khai cụ thể.
- **Mục đích**: Đảm bảo hàm đúng được gọi tại thời gian chạy, bất kể loại con trỏ hoặc tham chiếu, đạt được **đa hình thời gian chạy**.
- **Cú pháp**:
  ```cpp
  class Base {
  public:
      virtual return_type functionName() {
          // Nội dung hàm
      }
  };
  class Derived : public Base {
  public:
      return_type functionName() override {
          // Nội dung hàm được ghi đè
      }
  };
  ```
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void display() {
          cout << "Hiển thị lớp cơ sở" << endl;
      }
  };

  class Derived : public Base {
  public:
      void display() override {
          cout << "Hiển thị lớp dẫn xuất" << endl;
      }
  };

  int main() {
      Base* basePtr;
      Derived derivedObj;
      basePtr = &derivedObj;
      basePtr->display(); // Gọi display của Derived
      return 0;
  }
  ```
  **Kết quả**:
  ```
  Hiển thị lớp dẫn xuất
  ```

- **Điểm chính**:
  - Sử dụng định danh `override` trong lớp dẫn xuất để đảm bảo hàm ghi đè chính xác hàm ảo của lớp cơ sở và tránh lỗi.
  - Hàm ảo được giải quyết tại thời gian chạy thông qua cơ chế gọi là **liên kết muộn** (late binding).
  - Nếu lớp dẫn xuất không ghi đè hàm ảo, phiên bản của lớp cơ sở được sử dụng.

## Hàm Ảo Thuần Túy

- **Định nghĩa**: Hàm ảo thuần túy là hàm ảo được khai báo trong lớp cơ sở mà không có triển khai, được đánh dấu bằng `= 0`. Lớp chứa hàm ảo thuần túy là **lớp trừu tượng** và không thể khởi tạo.
- **Cú pháp**:
  ```cpp
  virtual return_type functionName() = 0;
  ```
- **Mục đích**: Buộc các lớp dẫn xuất cung cấp triển khai, đảm bảo giao diện nhất quán trong hệ thống phân cấp lớp.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void display() = 0; // Hàm ảo thuần túy
  };

  class Derived : public Base {
  public:
      void display() override {
          cout << "Hiển thị lớp dẫn xuất" << endl;
      }
  };

  int main() {
      Base* basePtr;
      Derived derivedObj;
      basePtr = &derivedObj;
      basePtr->display(); // Gọi display của Derived
      return 0;
  }
  ```
  **Kết quả**:
  ```
  Hiển thị lớp dẫn xuất
  ```

- **Điểm chính**:
  - Các lớp trừu tượng với hàm ảo thuần túy hoạt động như giao diện hoặc lớp cơ sở cho đa hình.
  - Lớp dẫn xuất phải ghi đè tất cả các hàm ảo thuần túy, nếu không chúng cũng trở thành lớp trừu tượng.

## Liên Kết Sớm và Liên Kết Muộn

- **Liên Kết Sớm (Early Binding)**: Lời gọi hàm được giải quyết tại thời điểm biên dịch dựa trên loại con trỏ hoặc tham chiếu. Được sử dụng cho các hàm không ảo và nhanh hơn.
- **Liên Kết Muộn (Late Binding)**: Lời gọi hàm được giải quyết tại thời gian chạy dựa trên loại đối tượng thực tế, sử dụng hàm ảo. Chậm hơn nhưng hỗ trợ đa hình.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      virtual void print() {
          cout << "In lớp cơ sở\n";
      }
      void show() {
          cout << "Hiển thị lớp cơ sở\n";
      }
  };

  class Derived : public Base {
  public:
      void print() override {
          cout << "In lớp dẫn xuất\n";
      }
      void show() {
          cout << "Hiển thị lớp dẫn xuất\n";
      }
  };

  int main() {
      Base* bptr;
      Derived d;
      bptr = &d;
      bptr->print(); // Liên kết muộn: gọi print của Derived
      bptr->show();  // Liên kết sớm: gọi show của Base
      return 0;
  }
  ```
  **Kết quả**:
  ```
  In lớp dẫn xuất
  Hiển thị lớp cơ sở
  ```

- **Giải thích**: Hàm `print()` là ảo, nên sử dụng liên kết muộn để gọi phiên bản của lớp dẫn xuất. Hàm `show()` không ảo, nên sử dụng liên kết sớm để gọi phiên bản của lớp cơ sở.

## Cách Hoạt Động của Hàm Ảo (VTABLE và VPTR)

- **Cơ chế**:
  - Khi một lớp chứa hàm ảo, trình biên dịch tạo một **VTABLE** (bảng ảo), một mảng tĩnh chứa các con trỏ đến các hàm ảo của lớp.
  - Mỗi đối tượng của lớp chứa một **VPTR** (con trỏ ảo), một thành viên dữ liệu ẩn trỏ đến VTABLE của lớp đó.
  - Trong lời gọi hàm ảo, VPTR được sử dụng để tra cứu địa chỉ hàm đúng trong VTABLE tại thời gian chạy.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Base {
  public:
      void fun_1() { cout << "cơ sở-1\n"; }
      virtual void fun_2() { cout << "cơ sở-2\n"; }
      virtual void fun_3() { cout << "cơ sở-3\n"; }
      virtual void fun_4() { cout << "cơ sở-4\n"; }
  };

  class Derived : public Base {
  public:
      void fun_1() { cout << "dẫn xuất-1\n"; }
      void fun_2() override { cout << "dẫn xuất-2\n"; }
      void fun_4(int x) { cout << "dẫn xuất-4\n"; }
  };

  int main() {
      Base* p;
      Derived obj1;
      p = &obj1;
      p->fun_1();    // Liên kết sớm: cơ sở-1
      p->fun_2();    // Liên kết muộn: dẫn xuất-2
      p->fun_3();    // Liên kết muộn: cơ sở-3
      p->fun_4();    // Liên kết muộn: cơ sở-4
      // p->fun_4(5); // CTE: fun_4(int) không có trong Base
      return 0;
  }
  ```
  **Kết quả**:
  ```
  cơ sở-1
  dẫn xuất-2
  cơ sở-3
  cơ sở-4
  ```

- **Giải thích**:
  - Trình biên dịch tạo VTABLE cho `Base` với các mục cho `fun_2`, `fun_3`, và `fun_4`, và VTABLE cho `Derived` với các mục được cập nhật cho hàm ghi đè (`fun_2`).
  - VPTR của đối tượng `Derived` trỏ đến VTABLE của `Derived`, cho phép giải quyết lời gọi hàm ảo tại thời gian chạy.
  - Hàm không ảo (`fun_1`) sử dụng liên kết sớm, và `fun_4(int)` trong `Derived` là một hàm khác (không phải ghi đè) do chữ ký khác.

## Quy Tắc cho Hàm Ảo
1. Hàm ảo **không thể là tĩnh** vì hàm tĩnh thuộc về lớp, không gắn với đối tượng.
2. Hàm ảo có thể là **hàm bạn** của một lớp khác.
3. Hàm ảo nên được truy cập bằng **con trỏ hoặc tham chiếu** của loại lớp cơ sở để đạt được đa hình thời gian chạy.
4. **Nguyên mẫu** (kiểu trả về, tên, và tham số) của hàm ảo phải giống nhau trong lớp cơ sở và lớp dẫn xuất.
5. Hàm ảo được **định nghĩa trong lớp cơ sở** và có thể được ghi đè trong lớp dẫn xuất. Nếu không được ghi đè, phiên bản của lớp cơ sở được sử dụng.
6. Một lớp có thể có **hàm hủy ảo** để đảm bảo dọn dẹp đúng trong hệ thống phân cấp đa hình, nhưng **không thể có hàm tạo ảo** (hàm tạo không hỗ trợ đa hình).

## Hạn Chế của Hàm Ảo
- **Hiệu suất**: Lời gọi hàm ảo chậm hơn một chút do giải quyết tại thời gian chạy qua VTABLE, và khó tối ưu hóa tại thời điểm biên dịch.
- **Gỡ lỗi**: Trong các hệ thống phức tạp, việc theo dõi hàm ảo nào được gọi có thể khó khăn do liên kết thời gian chạy.

## Thực Tiễn Tốt Nhất
- Sử dụng định danh `override` trong lớp dẫn xuất để đảm bảo ghi đè đúng và phát hiện lỗi tại thời điểm biên dịch.
- Khai báo hàm hủy là ảo trong lớp cơ sở để đảm bảo dọn dẹp đúng trong hệ thống phân cấp đa hình.
- Sử dụng hàm ảo thuần túy để định nghĩa giao diện trong các lớp trừu tượng.
- Hạn chế sử dụng hàm ảo nếu hiệu suất là yếu tố quan trọng và không cần đa hình.

## Tóm Tắt
Từ khóa `virtual` trong C++ hỗ trợ đa hình thời gian chạy bằng cách cho phép lớp dẫn xuất ghi đè các hàm của lớp cơ sở. Hàm ảo được giải quyết tại thời gian chạy thông qua VTABLE và VPTR, đảm bảo gọi đúng hàm dựa trên loại đối tượng thực tế. Hàm ảo thuần túy tạo ra các lớp trừu tượng, bắt buộc lớp dẫn xuất triển khai giao diện. Hiểu rõ hàm ảo là cần thiết để thiết kế các chương trình C++ linh hoạt và dễ mở rộng.