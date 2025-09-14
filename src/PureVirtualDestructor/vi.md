# Hàm Hủy Thuần Ảo Trong C++

Trong C++, **hàm hủy thuần ảo** (pure virtual destructor) là một hàm hủy được khai báo là thuần ảo trong lớp cơ sở, khiến lớp trở thành trừu tượng đồng thời đảm bảo dọn dẹp đúng các đối tượng lớp dẫn xuất trong hệ thống phân cấp đa hình. Tài liệu từ GeeksforGeeks (https://www.geeksforgeeks.org/pure-virtual-destructor-c/) giải thích rằng hàm hủy thuần ảo là hợp lệ trong C++ nhưng yêu cầu một thân hàm, không giống các hàm thuần ảo khác, do cách gọi đặc biệt của hàm hủy. Bài viết này làm rõ khái niệm, cách triển khai, lý do cần thân hàm, và kết nối với các khái niệm C++ liên quan như hàm hủy ảo, hàm hủy riêng, và hàm tạo sao chép.

## Hàm Hủy Thuần Ảo Là Gì?

- **Định nghĩa**: Hàm hủy thuần ảo là hàm hủy được khai báo với đặc tả `= 0` trong lớp cơ sở, khiến nó trở thành thuần ảo và biến lớp thành trừu tượng (tức là không thể tạo trực tiếp các đối tượng của lớp). Không giống các hàm thuần ảo khác, hàm hủy thuần ảo phải có thân hàm được cung cấp ngoài khai báo lớp.
- **Cú pháp**:
  ```cpp
  class Base {
  public:
      virtual ~Base() = 0; // Hàm hủy thuần ảo
  };
  Base::~Base() { // Thân hàm bắt buộc
      // Mã dọn dẹp
  }
  ```
- **Mục đích**:
  - Đảm bảo dọn dẹp đúng các đối tượng lớp dẫn xuất khi được xóa thông qua con trỏ lớp cơ sở, tương tự như hàm hủy ảo thông thường.
  - Biến lớp thành trừu tượng, ngăn chặn việc tạo trực tiếp, hữu ích cho việc định nghĩa giao diện hoặc lớp cơ sở trừu tượng.
- **Sự Khác Biệt Chính So Với Hàm Thuần Ảo Khác**: Trong khi các hàm thuần ảo khác (ví dụ: `virtual void func() = 0`) thường không yêu cầu thân hàm, hàm hủy thuần ảo phải có thân hàm vì các hàm hủy được gọi theo thứ tự ngược lại (từ dẫn xuất đến cơ sở) trong quá trình hủy đối tượng.

## Tại Sao Cần Thân Hàm?

- **Thứ Tự Gọi Hàm Hủy**:
  - Trong C++, các hàm hủy không được ghi đè như các hàm ảo thông thường. Thay vào đó, chúng được gọi đệ quy theo thứ tự ngược lại của việc xây dựng: hàm hủy của lớp dẫn xuất được gọi trước, sau đó là hàm hủy của lớp cơ sở.
  - Nếu hàm hủy thuần ảo thiếu thân hàm, sẽ không có mã nào để thực thi khi hàm hủy của lớp cơ sở được gọi, dẫn đến lỗi liên kết (undefined reference).
- **Thực Thi Liên Kết**:
  - Trình biên dịch và trình liên kết yêu cầu một thân hàm cho hàm hủy thuần ảo để đảm bảo rằng phần lớp cơ sở của đối tượng được dọn dẹp đúng cách trong quá trình hủy.
- **Tính Chất Lớp Trừu Tượng**:
  - Việc khai báo hàm hủy là thuần ảo khiến lớp trở thành trừu tượng, nghĩa là không thể tạo trực tiếp các đối tượng. Điều này hữu ích cho việc định nghĩa các giao diện mà các lớp dẫn xuất phải cung cấp triển khai riêng của các hàm thuần ảo khác.

## Ví Dụ: Hàm Hủy Thuần Ảo Không Có Thân Hàm (Lỗi Liên Kết)

Tài liệu cung cấp một ví dụ thất bại do thiếu thân hàm:

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual ~Base() = 0; // Hàm hủy thuần ảo, không có thân hàm
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "~Derived() được thực thi\n";
    }
};

int main() {
    Base* b = new Derived();
    delete b;
    return 0;
}
```

**Kết Quả** (Lỗi Liên Kết):
```
undefined reference to `Base::~Base()'
error: ld returned 1 exit status
```

- **Giải Thích**:
  - Chương trình cố gắng xóa một đối tượng `Derived` thông qua con trỏ `Base`.
  - Hàm hủy `Derived` được gọi trước, nhưng trình liên kết không thể tìm thấy thân hàm cho hàm hủy `Base`, dẫn đến lỗi.
  - Điều này nhấn mạnh sự cần thiết của việc cung cấp thân hàm cho hàm hủy thuần ảo.

## Ví Dụ: Hàm Hủy Thuần Ảo Có Thân Hàm (Hành Vi Đúng)

Tài liệu cung cấp phiên bản sửa lỗi:

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual ~Base() = 0; // Hàm hủy thuần ảo
};

Base::~Base() { // Thân hàm được cung cấp
    cout << "Hàm hủy thuần ảo được gọi\n";
}

class Derived : public Base {
public:
    ~Derived() {
        cout << "~Derived() được thực thi\n";
    }
};

int main() {
    Base* b = new Derived();
    delete b;
    return 0;
}
```

**Kết Quả**:
```
~Derived() được thực thi
Hàm hủy thuần ảo được gọi
```

- **Giải Thích**:
  - Đối tượng `Derived` được tạo, và hàm tạo của nó được gọi (ngầm định, vì không có hàm tạo nào được hiển thị).
  - Khi `delete b` được gọi, cơ chế hàm hủy ảo đảm bảo rằng hàm hủy `Derived` được gọi trước, tiếp theo là hàm hủy `Base`.
  - Thân hàm được cung cấp cho `Base::~Base()` được thực thi, đảm bảo dọn dẹp đúng và không có lỗi liên kết.
  - Lớp `Base` là trừu tượng do hàm hủy thuần ảo, ngăn chặn việc tạo trực tiếp (`Base b;` sẽ thất bại).

## Ví Dụ: Lớp Trừu Tượng Với Hàm Hủy Thuần Ảo

Tài liệu minh họa rằng một lớp có hàm hủy thuần ảo trở thành trừu tượng:

```cpp
#include <iostream>
using namespace std;

class Test {
public:
    virtual ~Test() = 0; // Hàm hủy thuần ảo
};

Test::~Test() {}

int main() {
    Test p; // Lỗi: không thể tạo đối tượng của lớp trừu tượng
    Test* t1 = new Test; // Lỗi: không thể tạo đối tượng của lớp trừu tượng
    return 0;
}
```

**Kết Quả** (Lỗi Biên Dịch):
```
[Error] cannot declare variable 'p' to be of abstract type 'Test'
[Note] because the following virtual functionsNo space left after 'because the following virtual functions are pure within 'Test':'
[Note] virtual Test::~Test()
[Error] cannot allocate an object of abstract type 'Test'
[Note] since type 'Test' has pure virtual functions
```

- **Giải Thích**:
  - Lớp `Test` là trừu tượng vì nó chứa một hàm hủy thuần ảo.
  - Các nỗ lực tạo đối tượng (`Test p;` hoặc `Test* t1 = new Test;`) thất bại vì các lớp trừu tượng không thể được tạo trực tiếp.
  - Các lớp dẫn xuất phải cung cấp hàm hủy riêng, và thân hàm hủy cơ sở đảm bảo dọn dẹp đúng cách.

## Kết Nối Với Các Chủ Đề Trước

- **Hàm Tạo vs. Hàm Thành Viên** (từ phản hồi trước):
  - Giống như hàm tạo, hàm hủy là hàm thành viên đặc biệt không có kiểu trả về và được gọi tự động. Hàm hủy thuần ảo mở rộng điều này bằng cách biến lớp thành trừu tượng đồng thời đảm bảo dọn dẹp đa hình.
- **Hàm Tạo Sao Chép và Toán Tử Gán** (từ phản hồi trước):
  - Trong các lớp có bộ nhớ động, một hàm hủy thuần ảo đảm bảo dọn dẹp đúng khi xóa đa hình. Một hàm tạo sao chép sâu (như đã thảo luận trong sao chép nông vs. sâu) bổ sung cho điều này bằng cách đảm bảo các đối tượng được sao chép là độc lập, tránh xung đột tài nguyên.
- **Hàm Tạo Ảo** (từ phản hồi trước):
  - Hàm tạo không thể là ảo do thiếu bảng ảo trong quá trình xây dựng đối tượng. Ngược lại, hàm hủy có thể là ảo hoặc thuần ảo vì bảng ảo có sẵn trong quá trình hủy, cho phép hành vi đa hình.
- **Hàm Tạo Sao Chép Ảo** (từ phản hồi trước):
  - Phương thức `Clone` ảo dựa vào hàm tạo sao chép để tạo bản sao đa hình. Một hàm hủy thuần ảo trong lớp cơ sở đảm bảo rằng các đối tượng được sao chép này được dọn dẹp đúng cách khi xóa thông qua con trỏ lớp cơ sở.
- **Hàm Hủy Riêng** (từ phản hồi trước):
  - Một hàm hủy riêng hạn chế việc hủy chỉ trong phạm vi lớp hoặc các hàm bạn. Nó cũng có thể là thuần ảo, khiến lớp trở thành trừu tượng đồng thời yêu cầu hàm bạn hoặc phương thức lớp để xóa, như đã thảo luận trước đây.
- **Hàm Hủy Ảo** (từ phản hồi trước):
  - Hàm hủy thuần ảo là một trường hợp đặc biệt của hàm hủy ảo. Trong khi hàm hủy ảo thông thường đảm bảo dọn dẹp đúng trong các hệ thống phân cấp đa hình, hàm hủy thuần ảo còn làm lớp trở thành trừu tượng, buộc phải dẫn xuất.
- **Sao Chép Nông vs. Sâu**:
  - Trong các lớp có bộ nhớ động, một hàm hủy thuần ảo đảm bảo rằng các tài nguyên được giải phóng trong quá trình xóa đa hình. Một hàm tạo sao chép sâu là cần thiết để tránh các vấn đề về tài nguyên chia sẻ khi sao chép các đối tượng, như đã thảo luận trước đây.

## Các Điểm Quan Trọng

- **Hợp Lệ Trong C++**:
  - Hàm hủy thuần ảo là hợp lệ và hữu ích cho việc định nghĩa các lớp cơ sở trừu tượng cần dọn dẹp đa hình.
- **Yêu Cầu Thân Hàm**:
  - Không giống các hàm thuần ảo khác, hàm hủy thuần ảo phải có thân hàm để tránh lỗi liên kết, vì các hàm hủy được gọi đệ quy từ dẫn xuất đến cơ sở.
- **Lớp Trừu Tượng**:
  - Một lớp có hàm hủy thuần ảo là trừu tượng, ngăn chặn việc tạo trực tiếp. Các lớp dẫn xuất phải cung cấp hàm hủy riêng của chúng.
- **Dọn Dẹp Đa Hình**:
  - Giống như hàm hủy ảo thông thường, hàm hủy thuần ảo đảm bảo rằng các hàm hủy của lớp dẫn xuất được gọi khi xóa thông qua con trỏ lớp cơ sở, tránh hành vi không xác định.
- **Cơ Chế Bảng Ảo (Vtable)**:
  - Trình biên dịch sử dụng bảng ảo để gọi đúng hàm hủy tại thời gian chạy, với một con trỏ ẩn (`vptr`) trong mỗi đối tượng trỏ đến bảng ảo của lớp.

## Thực Tiễn Tốt Nhất

- **Luôn Cung Cấp Thân Hàm**:
  - Định nghĩa một thân hàm cho hàm hủy thuần ảo bên ngoài khai báo lớp để tránh lỗi liên kết (`undefined reference`).
- **Sử Dụng Trong Các Lớp Cơ Sở Trừu Tượng**:
  - Sử dụng hàm hủy thuần ảo trong các lớp được thiết kế làm giao diện hoặc lớp cơ sở trừu tượng để buộc dẫn xuất và đảm bảo dọn dẹp đa hình.
- **Kết Hợp Với Sao Chép Sâu**:
  - Đối với các lớp có bộ nhớ động, triển khai các hàm tạo sao chép sâu và toán tử gán để bổ sung cho hàm hủy thuần ảo, đảm bảo tính độc lập của tài nguyên.
- **Xử Lý Hàm Hủy Riêng Cẩn Thận**:
  - Nếu một hàm hủy thuần ảo là riêng (như đã thảo luận trong phản hồi về hàm hủy riêng), cung cấp các hàm bạn hoặc phương thức lớp để xóa có kiểm soát trong các hệ thống phân cấp đa hình.
- **Kiểm Tra Việc Xóa Đa Hình**:
  - Xác minh rằng việc xóa các đối tượng thông qua con trỏ lớp cơ sở gọi đúng các hàm hủy của lớp dẫn xuất, đảm bảo dọn dẹp đúng trong các hệ thống phân cấp phức tạp.
- **Ghi Lại Ý Định**:
  - Ghi lại rõ ràng việc sử dụng hàm hủy thuần ảo để chỉ ra vai trò của chúng trong việc biến lớp thành trừu tượng và đảm bảo dọn dẹp đa hình, hỗ trợ bảo trì và hợp tác.
- **Tránh Hàm Hủy Thuần Ảo Không Cần Thiết**:
  - Chỉ sử dụng hàm hủy thuần ảo khi lớp phải là trừu tượng. Nếu chỉ cần đa hình mà không cần trừu tượng, hàm hủy ảo thông thường là đủ.

## Tóm Tắt

Hàm hủy thuần ảo trong C++ là một tính năng mạnh mẽ để định nghĩa các lớp cơ sở trừu tượng đồng thời đảm bảo dọn dẹp đúng trong các hệ thống phân cấp đa hình. Bằng cách khai báo hàm hủy là thuần ảo với một thân hàm được cung cấp, lớp trở thành trừu tượng, ngăn chặn việc tạo trực tiếp, và đảm bảo rằng các hàm hủy của lớp dẫn xuất được gọi khi xóa thông qua con trỏ lớp cơ sở. Yêu cầu về thân hàm phân biệt hàm hủy thuần ảo với các hàm thuần ảo khác, giải quyết tính chất đệ quy của các cuộc gọi hàm hủy. Khái niệm này tích hợp với các tính năng C++ khác như sao chép sâu, hàm hủy ảo, và hàm hủy riêng, tạo thành một phần quan trọng của thiết kế hướng đối tượng mạnh mẽ trong C++.