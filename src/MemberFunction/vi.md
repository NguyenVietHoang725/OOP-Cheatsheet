# Các Loại Hàm Thành Viên Trong C++

Trong C++, **hàm thành viên** là các hàm được định nghĩa bên trong một lớp, hoạt động trên các đối tượng của lớp đó. Chúng định nghĩa hành vi của lớp và có thể truy cập các thành viên dữ liệu của nó. Ngoài các hàm thành viên cơ bản, C++ hỗ trợ một số loại hàm thành viên đặc biệt, mỗi loại có đặc điểm và trường hợp sử dụng riêng. Dưới đây là tổng quan toàn diện về các loại hàm thành viên trong C++.

## 1. Hàm Thành Viên Đơn Giản
- **Định nghĩa**: Đây là các hàm thành viên tiêu chuẩn không có từ khóa đặc biệt (như `static`, `const`, v.v.). Chúng hoạt động trên một đối tượng cụ thể của lớp và có thể truy cập tất cả các thành viên của nó.
- **Cú pháp**:
  ```cpp
  return_type functionName(parameter_list) {
      // Nội dung hàm
  }
  ```
- **Trường hợp sử dụng**: Dùng cho các thao tác chung như thiết lập hoặc lấy dữ liệu hoặc thực hiện các phép tính cụ thể cho một đối tượng.
- **Ví dụ**:
  ```cpp
  class Example {
  public:
      int x;
      void setX(int val) {
          x = val; // Sửa đổi dữ liệu của đối tượng
      }
  };
  int main() {
      Example obj;
      obj.setX(10); // Gọi hàm thành viên đơn giản
      cout << obj.x; // Kết quả: 10
      return 0;
  }
  ```

## 2. Hàm Thành Viên Tĩnh (Static)
- **Định nghĩa**: Được khai báo với từ khóa `static`, các hàm này thuộc về lớp thay vì bất kỳ đối tượng cụ thể nào. Chúng không thể truy cập trực tiếp các thành viên không tĩnh và không có con trỏ `this`.
- **Truy cập**: Được gọi bằng tên lớp và toán tử phân giải phạm vi (`::`), mặc dù cũng có thể gọi qua một đối tượng.
- **Cú pháp**:
  ```cpp
  class Example {
  public:
      static void staticFunc() {
          cout << "Hàm tĩnh được gọi";
      }
  };
  int main() {
      Example::staticFunc(); // Gọi bằng tên lớp
      return 0;
  }
  ```
- **Trường hợp sử dụng**: Dùng cho các thao tác áp dụng cho toàn bộ lớp, như các hàm tiện ích hoặc truy cập các thành viên dữ liệu tĩnh.
- **Điểm chính**:
  - Chỉ có thể truy cập các thành viên dữ liệu `static` và các hàm thành viên `static` khác.
  - Không có con trỏ `this`, vì chúng không gắn với một đối tượng.

## 3. Hàm Thành Viên Hằng (Const)
- **Định nghĩa**: Được khai báo với từ khóa `const` sau chữ ký hàm, các hàm này cam kết không sửa đổi trạng thái của đối tượng (các thành viên dữ liệu) hoặc gọi các hàm thành viên không phải const.
- **Cú pháp**:
  ```cpp
  return_type functionName() const {
      // Nội dung hàm (không thể sửa đổi đối tượng)
  }
  ```
- **Trường hợp sử dụng**: Dùng để đảm bảo hàm chỉ đọc dữ liệu của đối tượng, thường dùng cho các phương thức lấy dữ liệu hoặc kiểm tra.
- **Ví dụ**:
  ```cpp
  class Example {
      int x;
  public:
      Example(int val) : x(val) {}
      int getX() const {
          // x = 20; // Lỗi: không thể sửa đổi x trong hàm const
          return x;
      }
  };
  int main() {
      Example obj(10);
      cout << obj.getX(); // Kết quả: 10
      return 0;
  }
  ```

## 4. Hàm Thành Viên Nội Tuyến (Inline)
- **Định nghĩa**: Các hàm được định nghĩa bên trong định nghĩa lớp là **ngầm định nội tuyến**. Bạn cũng có thể khai báo hàm là `inline` một cách rõ ràng để gợi ý cho trình biên dịch thay thế các lời gọi hàm bằng nội dung hàm nhằm tối ưu hóa hiệu suất.
- **Cú pháp**:
  ```cpp
  class Example {
  public:
      inline void func() { // Rõ ràng nội tuyến
          cout << "Hàm nội tuyến";
      }
      void autoInline() { // Ngầm định nội tuyến (định nghĩa trong lớp)
          cout << "Hàm tự động nội tuyến";
      }
  };
  ```
- **Trường hợp sử dụng**: Dùng cho các hàm nhỏ, được gọi thường xuyên để giảm chi phí gọi hàm.
- **Điểm chính**:
  - Nội tuyến là một gợi ý tối ưu hóa của trình biên dịch, không phải là bắt buộc.
  - Lạm dụng hàm nội tuyến có thể làm tăng kích thước mã.

## 5. Hàm Bạn (Friend Functions)
- **Định nghĩa**: Được khai báo với từ khóa `friend`, đây **không phải là hàm thành viên** nhưng được cấp quyền truy cập vào các thành viên `private` và `protected` của lớp. Chúng có thể là hàm toàn cục, hàm thành viên của lớp khác, hoặc toàn bộ một lớp.
- **Cú pháp**:
  ```cpp
  class Example {
      int x;
  public:
      Example(int val) : x(val) {}
      friend void print(Example&); // Hàm bạn
  };
  void print(Example& e) {
      cout << e.x; // Truy cập thành viên private
  }
  ```
- **Trường hợp sử dụng**: Dùng cho các thao tác yêu cầu truy cập vào các thành viên private, như nạp chồng toán tử hoặc khi hai lớp cần liên kết chặt chẽ.
- **Ví dụ**:
  ```cpp
  class Other {
  public:
      void access();
  };
  class Example {
      int x;
  public:
      Example(int val) : x(val) {}
      friend void Other::access(); // Hàm bạn của lớp khác
      friend class Other; // Toàn bộ lớp là bạn
  };
  ```
- **Điểm chính**:
  - Vi phạm đóng gói, nên sử dụng một cách tiết chế.
  - Tính bạn không được kế thừa, không đối xứng, và không chuyển tiếp.
  - Một lớp bạn sẽ tự động khiến tất cả các hàm thành viên của nó trở thành hàm bạn.

## Ghi Chú Quan Trọng
- **Đóng Gói**: Hàm `friend` có thể phá vỡ đóng gói, điều này gây tranh cãi trong thiết kế OOP thuần túy.
- **Tĩnh vs. Thể Hiện**: Hàm tĩnh hoạt động ở cấp độ lớp, trong khi các hàm khác (đơn giản, const, nội tuyến) hoạt động trên các đối tượng cụ thể.
- **An Toàn Const**: Hàm const đảm bảo tính bất biến, rất quan trọng cho các giao diện an toàn với các đối tượng const.
- **Hiệu Suất**: Hàm nội tuyến nhằm cải thiện hiệu suất nhưng nên được sử dụng cẩn thận cho các hàm nhỏ.
- **Linh Hoạt**: C++ cho phép kết hợp các loại hàm này để phù hợp với các nhu cầu thiết kế khác nhau, cân bằng giữa tính linh hoạt và đóng gói.