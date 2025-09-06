# Từ Khóa `static` Trong C++

Trong C++, từ khóa `static` có nhiều ý nghĩa tùy thuộc vào ngữ cảnh sử dụng, chủ yếu được dùng để kiểm soát thời gian tồn tại, phạm vi, và khả năng truy cập của biến và hàm. Đây là một công cụ linh hoạt hỗ trợ các tính năng như lưu trữ bền vững, dữ liệu chia sẻ, và chức năng cấp lớp. Bài viết này giải thích các cách sử dụng của từ khóa `static` với các ví dụ và ứng dụng.

## Các Cách Sử Dụng Từ Khóa `static`

### 1. Biến Static Trong Hàm
- **Định nghĩa**: Một biến static được khai báo trong một hàm sẽ giữ giá trị của nó giữa các lần gọi hàm và được cấp phát bộ nhớ trong suốt thời gian tồn tại của chương trình.
- **Đặc điểm**:
  - Chỉ được khởi tạo một lần, khi hàm được gọi lần đầu.
  - Bộ nhớ được cấp phát trong vùng lưu trữ tĩnh riêng biệt, không phải trên stack.
  - Giá trị của biến được duy trì qua các lần gọi hàm.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  void f() {
      static int count = 0; // Biến static
      count++;
      cout << count << " ";
  }

  int main() {
      for (int i = 0; i < 5; i++) {
          f(); // Gọi hàm nhiều lần
      }
      return 0;
  }
  ```
  **Kết quả**:
  ```
  1 2 3 4 5
  ```
- **Giải thích**: Biến `count` được khởi tạo một lần và giữ giá trị, tăng dần qua mỗi lần gọi hàm `f()`.
- **Ứng dụng**:
  - Lưu trữ trạng thái trước đó cho các coroutine hoặc memoization trong hàm đệ quy.
  - Trả về địa chỉ của biến cục bộ một cách an toàn, vì nó tồn tại ngoài phạm vi hàm.
  - Theo dõi số lần gọi hàm.

### 2. Thành Viên Dữ Liệu Static Trong Lớp
- **Định nghĩa**: Một thành viên dữ liệu static trong lớp được chia sẻ bởi tất cả các đối tượng của lớp, chỉ có một bản sao duy nhất trong bộ nhớ bất kể số lượng đối tượng.
- **Đặc điểm**:
  - Được cấp phát trong vùng lưu trữ tĩnh và chỉ khởi tạo một lần.
  - Phải được khởi tạo rõ ràng bên ngoài lớp bằng toán tử phân giải phạm vi (`::`).
  - Không thể khởi tạo trong hàm tạo, vì nó không gắn với đối tượng cụ thể.
  - Có thể truy cập mà không cần tạo đối tượng của lớp.
- **Ví dụ** (Khởi tạo sai):
  ```cpp
  #include <iostream>
  using namespace std;

  class GfG {
  public:
      static int i;
      GfG() {}
  };

  int main() {
      GfG obj1, obj2;
      obj1.i = 2;
      obj2.i = 3;
      cout << obj1.i << " " << obj2.i; // Hành vi không xác định
      return 0;
  }
  ```
  **Kết quả**: Lỗi biên dịch (`undefined reference to GfG::i`), vì `i` chưa được khởi tạo.

- **Ví dụ đúng**:
  ```cpp
  #include <iostream>
  using namespace std;

  class GfG {
  public:
      static int i;
      GfG() {}
  };

  int GfG::i = 1; // Khởi tạo thành viên static

  int main() {
      cout << GfG::i; // Kết quả: 1
      return 0;
  }
  ```
  **Kết quả**:
  ```
  1
  ```
- **Giải thích**: Thành viên static `i` được khởi tạo bên ngoài lớp và có thể truy cập trực tiếp bằng tên lớp.
- **Ứng dụng**:
  - Đếm số lượng đối tượng được tạo của một lớp.
  - Lưu trữ cấu hình hoặc cài đặt chung.
  - Quản lý tài nguyên chia sẻ giữa các đối tượng.
  - Triển khai mẫu singleton để đảm bảo chỉ có một thể hiện của lớp.

### 3. Hàm Thành Viên Static Trong Lớp
- **Định nghĩa**: Hàm thành viên static thuộc về lớp, không phụ thuộc vào bất kỳ đối tượng cụ thể nào và chỉ có thể truy cập các thành viên dữ liệu static hoặc các hàm thành viên static khác.
- **Đặc điểm**:
  - Có thể được gọi bằng tên lớp và toán tử phân giải phạm vi (`::`), không cần tạo đối tượng.
  - Không thể truy cập các thành viên không static, vì chúng không gắn với thể hiện đối tượng.
  - Thường dùng cho các hàm tiện ích hoặc hỗ trợ liên quan đến lớp.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class GfG {
  public:
      static void printMsg() {
          cout << "Chào mừng đến với GfG!";
      }
  };

  int main() {
      GfG::printMsg(); // Gọi mà không cần đối tượng
      return 0;
  }
  ```
  **Kết quả**:
  ```
  Chào mừng đến với GfG!
  ```
- **Ứng dụng**:
  - Truy cập hoặc sửa đổi các thành viên dữ liệu static.
  - Triển khai các hàm hỗ trợ không phụ thuộc vào thể hiện đối tượng.
  - Hỗ trợ mẫu singleton hoặc các phương thức factory để tạo đối tượng.
  - Ghi log hoặc gỡ lỗi ở cấp độ lớp.

### 4. Biến Static Toàn Cục
- **Định nghĩa**: Một biến static toàn cục được khai báo bên ngoài bất kỳ lớp hoặc hàm nào với từ khóa `static`, giới hạn phạm vi của nó trong tệp (đơn vị dịch) nơi nó được định nghĩa (liên kết nội bộ).
- **Đặc điểm**:
  - Chỉ có thể truy cập trong tệp nơi nó được khai báo, ngăn chặn xung đột tên với các tệp khác.
  - Chỉ được khởi tạo một lần và tồn tại trong suốt thời gian chạy của chương trình.
  - Hoạt động như biến toàn cục nhưng với phạm vi hạn chế.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  static int count = 0; // Biến static toàn cục

  void increment() {
      count++;
      cout << count << " ";
  }

  int main() {
      increment();
      increment();
      return 0;
  }
  ```
  **Kết quả**:
  ```
  1 2
  ```
- **Ứng dụng**:
  - Giới hạn phạm vi biến trong một tệp để tránh xung đột.
  - Duy trì các bộ đếm hoặc cờ toàn cục trong tệp.
  - Lưu trữ cài đặt hoặc tài nguyên cụ thể cho tệp.
  - Quản lý trạng thái chia sẻ giữa các hàm trong một tệp.

## Ghi Chú Quan Trọng
- **Thời Gian Tồn Tại và Phạm Vi**:
  - Biến static (trong hàm hoặc toàn cục) tồn tại trong suốt thời gian chạy của chương trình.
  - Thành viên dữ liệu static và biến static toàn cục có liên kết nội bộ theo mặc định, trừ khi được khai báo là liên kết bên ngoài.
- **Khởi Tạo**:
  - Biến static được khởi tạo về 0 theo mặc định nếu không được khởi tạo rõ ràng.
  - Thành viên dữ liệu static phải được khởi tạo bên ngoài định nghĩa lớp.
- **Hạn Chế**:
  - Hàm thành viên static không thể truy cập các thành viên không static.
  - Biến static cục bộ không được phép trong Java, khác với C++.
- **Thực Tiễn Tốt Nhất**:
  - Sử dụng biến và hàm static cho dữ liệu và thao tác chia sẻ hoặc bền vững.
  - Khởi tạo thành viên dữ liệu static rõ ràng để tránh hành vi không xác định.
  - Ưu tiên truy cập bằng tên lớp (`ClassName::member`) cho các thành viên static để tăng tính rõ ràng.
  - Sử dụng biến static toàn cục để đóng gói dữ liệu cụ thể cho tệp và tránh ô nhiễm không gian tên toàn cục.

## Tóm Tắt
Từ khóa `static` trong C++ là một công cụ linh hoạt điều chỉnh hành vi của biến và hàm trong các ngữ cảnh khác nhau. Nó hỗ trợ lưu trữ bền vững trong hàm, dữ liệu chia sẻ trong lớp, chức năng cấp lớp, và biến toàn cục giới hạn trong tệp. Hiểu rõ các ứng dụng và hạn chế của nó là rất quan trọng để thiết kế các chương trình C++ hiệu quả và dễ bảo trì.