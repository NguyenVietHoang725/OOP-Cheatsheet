# Khi Nào Nên Sử Dụng Struct Thay Vì Class Trong C++

Trong C++, sự khác biệt kỹ thuật giữa `struct` và `class` rất nhỏ: `struct` có các thành viên **public** theo mặc định, trong khi `class` có các thành viên **private** theo mặc định. Tuy nhiên, quyết định sử dụng `struct` thay vì `class` thường phụ thuộc vào **quy ước**, **ý định**, và **tính dễ đọc của mã**. Dưới đây là tóm tắt khi nào nên ưu tiên sử dụng `struct` thay vì `class`.

## Hướng Dẫn Sử Dụng `struct` Thay Vì `class`

1. **Tập Hợp Dữ Liệu Đơn Giản (Plain Old Data - POD)**:
   - Sử dụng `struct` khi cần một bộ chứa đơn giản để nhóm các dữ liệu liên quan mà không có hành vi phức tạp hoặc đóng gói.
   - Ví dụ: Biểu diễn một điểm trong không gian 3D.
     ```cpp
     struct Point3D {
         float x, y, z;
     };
     ```
   - **Lý do**: `struct` thể hiện rằng kiểu dữ liệu chủ yếu là một "tập hợp dữ liệu" với các thành viên công khai, giúp người đọc hiểu rõ rằng trọng tâm là dữ liệu, không phải hành vi.

2. **Tương Thích Với Mã C Hoặc Mã Cũ**:
   - Sử dụng `struct` khi làm việc với mã C hoặc thư viện C, vì C chỉ hỗ trợ `struct` cho các kiểu dữ liệu mà không có phương thức hoặc kiểm soát truy cập.
   - **Lý do**: Đảm bảo tính tương thích với mã kiểu C, nơi `struct` thường được sử dụng cho các cấu trúc dữ liệu.

3. **Không Có Bất Biến Hoặc Đóng Gói Tối Thiểu**:
   - Chọn `struct` khi các thành viên dữ liệu có thể thay đổi độc lập và không yêu cầu bất biến (điều kiện phải luôn đúng).
   - Ví dụ: Một struct cấu hình với các trường được thiết lập độc lập.
     ```cpp
     struct Config {
         int port;
         string hostname;
     };
     ```
   - **Lý do**: Không giống `class`, thường được dùng để thực thi bất biến thông qua các thành viên private và phương thức, `struct` cho thấy không có các ràng buộc như vậy.

4. **Tính Dễ Đọc Mã và Quy Ước**:
   - Sử dụng `struct` để tuân theo các quy ước mà các lập trình viên khác mong đợi cho các cấu trúc dữ liệu đơn giản, công khai.
   - Ví dụ: Trong các thư viện hoặc nhóm phát triển nơi `struct` được dùng cho dữ liệu công khai và `class` cho các đối tượng được đóng gói.
   - **Lý do**: Nó truyền đạt ý định rõ ràng, làm cho mã nguồn dễ hiểu hơn đối với người khác.

5. **Hiệu Suất Với Các Đối Tượng Giá Trị Nhỏ**:
   - Ưu tiên `struct` cho các kiểu dữ liệu nhỏ, bất biến, thường được sao chép hoặc sử dụng như kiểu giá trị (ví dụ: trong lập trình game cho các đỉnh).
   - Ví dụ: Một đỉnh trong mô hình 3D.
     ```cpp
     struct Vertex {
         float x, y, z;
         float u, v; // Tọa độ texture
     };
     ```
   - **Lý do**: `struct` thường được sử dụng cho các kiểu dữ liệu nhẹ với chi phí thấp, đặc biệt trong các ứng dụng yêu cầu hiệu suất cao.

## Khi Nào Nên Sử Dụng `class`
- Sử dụng `class` khi:
  - Cần **đóng gói** (các thành viên private với getter/setter).
  - Kiểu dữ liệu có **bất biến** cần được duy trì.
  - Kiểu dữ liệu mô hình hóa một **trừu tượng** phức tạp với hành vi (ví dụ: phương thức, hàm ảo).
  - Triển khai các tính năng **lập trình hướng đối tượng** như kế thừa hoặc đa hình.

## Những Điểm Cần Lưu Ý
- **Không Có Sự Khác Biệt Chức Năng**: Cả `struct` và `class` đều có thể có hàm thành viên, hàm tạo, hàm hủy, và kế thừa. Sự lựa chọn chủ yếu mang tính phong cách, không phải chức năng.
- **Hướng Dẫn C++ Core**:
  - Sử dụng `struct` để tổ chức dữ liệu liên quan mà không có bất biến.
  - Sử dụng `class` nếu kiểu dữ liệu có bất biến hoặc các thành viên không công khai.
- **Bối Cảnh Lịch Sử**: `struct` tồn tại trong C++ để tương thích ngược với C, nhưng vai trò của nó đã phát triển để hỗ trợ đầy đủ các tính năng giống như `class`. Sự lựa chọn thường phản ánh ý định của lập trình viên hơn là sự cần thiết về mặt kỹ thuật.

## Ví Dụ Minh Họa
- **Sử dụng `struct`**:
  ```cpp
  struct Employee {
      string name;
      int id;
      double salary;
  };
  ```
  - Phù hợp để nhóm dữ liệu đơn giản với truy cập công khai, không có logic phức tạp.
- **Sử dụng `class`**:
  ```cpp
  class BankAccount {
  private:
      double balance;
  public:
      void deposit(double amount);
      double getBalance() const;
  };
  ```
  - Phù hợp để đóng gói dữ liệu và hành vi với bất biến (ví dụ: số dư không thể âm).

## Kết Luận
Hãy chọn `struct` khi muốn nhấn mạnh một cấu trúc dữ liệu đơn giản, minh bạch với các thành viên công khai và không có hành vi hoặc bất biến phức tạp. Sử dụng `class` cho các kiểu yêu cầu đóng gói, bất biến, hoặc các tính năng lập trình hướng đối tượng. Quyết định này chủ yếu nhằm **truyền đạt ý định** tới các lập trình viên khác và tuân thủ các quy ước để đảm bảo mã dễ hiểu và dễ bảo trì.