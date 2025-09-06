# Từ Khóa `new` và `operator new` Trong C++

Trong C++, việc cấp phát bộ nhớ cho các đối tượng là một khía cạnh quan trọng của quản lý bộ nhớ động. Từ khóa `new` và `operator new` là hai cơ chế liên quan nhưng khác biệt được sử dụng để cấp phát bộ nhớ trên heap. Mặc dù chúng thường được sử dụng cùng nhau, chúng có mục đích khác nhau. Bài viết này giải thích về từ khóa `new`, `operator new`, và sự khác biệt giữa chúng, kèm theo các ví dụ minh họa.

## Từ Khóa `new`

- **Định nghĩa**: Từ khóa `new` là một toán tử yêu cầu cấp phát bộ nhớ trên heap và, đối với các đối tượng, gọi hàm tạo để khởi tạo bộ nhớ được cấp phát.
- **Chức năng**:
  - Cấp phát bộ nhớ bằng cách sử dụng `operator new`.
  - Gọi hàm tạo để khởi tạo đối tượng (nếu có).
  - Trả về con trỏ đến bộ nhớ được cấp phát và khởi tạo.
- **Trường hợp sử dụng**: Dùng để tạo đối tượng động, đặc biệt khi thời gian tồn tại hoặc kích thước của đối tượng được xác định tại thời điểm chạy.
- **Cú pháp**:
  ```cpp
  Type* pointer = new Type(arguments);
  ```
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Car {
      string name;
      int num;
  public:
      Car(string a, int n) {
          cout << "Hàm tạo được gọi" << endl;
          name = a;
          num = n;
      }
      void display() {
          cout << "Tên: " << name << endl;
          cout << "Số: " << num << endl;
      }
  };

  int main() {
      Car* p = new Car("Honda", 2017); // Cấp phát bộ nhớ và gọi hàm tạo
      p->display();
      delete p; // Giải phóng bộ nhớ
      return 0;
  }
  ```
  **Kết quả**:
  ```
  Hàm tạo được gọi
  Tên: Honda
  Số: 2017
  ```

- **Điểm chính**:
  - Từ khóa `new` xử lý cả việc cấp phát bộ nhớ và khởi tạo đối tượng.
  - Nếu không đủ bộ nhớ, nó ném ra ngoại lệ `std::bad_alloc` (trừ khi sử dụng phiên bản nothrow).
  - Được sử dụng cho cả kiểu dữ liệu cơ bản (ví dụ: `int* p = new int;`) và kiểu do người dùng định nghĩa (ví dụ: lớp).

## `operator new`

- **Định nghĩa**: `operator new` là một hàm cấp phát bộ nhớ thô trên heap, tương tự như `malloc()` trong C, nhưng không gọi hàm tạo.
- **Chức năng**:
  - Cấp phát một khối bộ nhớ chưa được khởi tạo với kích thước được chỉ định.
  - Trả về con trỏ `void*` đến bộ nhớ được cấp phát.
  - Có thể được nạp chồng ở phạm vi toàn cục hoặc cho một lớp cụ thể để tùy chỉnh việc cấp phát bộ nhớ.
- **Trường hợp sử dụng**: Dùng khi cần logic cấp phát bộ nhớ tùy chỉnh, chẳng hạn như sử dụng pool bộ nhớ hoặc gỡ lỗi cấp phát.
- **Cú pháp**:
  ```cpp
  void* operator new(size_t size);
  ```
- **Ví dụ** (Nạp chồng `operator new`):
  ```cpp
  #include <iostream>
  #include <cstdlib>
  using namespace std;

  class Car {
      string name;
      int num;
  public:
      Car(string a, int n) {
          cout << "Hàm tạo được gọi" << endl;
          name = a;
          num = n;
      }
      void display() {
          cout << "Tên: " << name << endl;
          cout << "Số: " << num << endl;
      }
      void* operator new(size_t size) {
          cout << "operator new được nạp chồng" << endl;
          void* p = malloc(size); // Cấp phát tùy chỉnh
          return p;
      }
      void operator delete(void* ptr) {
          cout << "operator delete được nạp chồng" << endl;
          free(ptr); // Giải phóng tùy chỉnh
      }
  };

  int main() {
      Car* p = new Car("HYUNDAI", 2012);
      p->display();
      delete p;
      return 0;
  }
  ```
  **Kết quả**:
  ```
  operator new được nạp chồng
  Hàm tạo được gọi
  Tên: HYUNDAI
  Số: 2012
  operator delete được nạp chồng
  ```

- **Điểm chính**:
  - `operator new` chỉ cấp phát bộ nhớ và không gọi hàm tạo. Hàm tạo được gọi tự động bởi từ khóa `new` sau khi `operator new` trả về.
  - Có thể nạp chồng ở phạm vi toàn cục hoặc cho một lớp cụ thể để triển khai các chiến lược cấp phát tùy chỉnh.
  - Nạp chồng `operator new` yêu cầu một `operator delete` tương ứng để xử lý giải phóng bộ nhớ nhất quán.

## Sự Khác Biệt Giữa Từ Khóa `new` và `operator new`

| Tính năng | Từ khóa `new` | `operator new` |
|-----------|---------------|----------------|
| **Loại** | Toán tử và từ khóa | Hàm |
| **Mục đích** | Cấp phát bộ nhớ và khởi tạo đối tượng (gọi hàm tạo) | Chỉ cấp phát bộ nhớ thô |
| **Gọi hàm tạo** | Tự động gọi hàm tạo | Không gọi hàm tạo |
| **Nạp chồng** | Không thể nạp chồng trực tiếp | Có thể nạp chồng ở phạm vi toàn cục hoặc cho lớp |
| **Cách sử dụng** | `Type* p = new Type(args);` | `void* p = operator new(sizeof(Type));` |
| **Kiểu trả về** | Con trỏ đến kiểu đối tượng (`Type*`) | `void*` |

- **Mối quan hệ**: Từ khóa `new` gọi nội bộ `operator new` để cấp phát bộ nhớ, sau đó gọi hàm tạo để khởi tạo đối tượng. Ví dụ, `new Car("Honda", 2017)` gọi `operator new(sizeof(Car))` rồi đến hàm tạo của `Car`.

## Ghi Chú Bổ Sung
- **Nạp Chồng `operator new`**:
  - Nạp chồng cho phép tùy chỉnh, như sử dụng pool bộ nhớ hoặc ghi nhật ký cấp phát.
  - Ví dụ nạp chồng toàn cục:
    ```cpp
    #include <iostream>
    #include <cstdlib>
    using namespace std;

    void* operator new(size_t size) {
        cout << "Nạp chồng operator new toàn cục, kích thước: " << size << endl;
        return malloc(size);
    }

    void operator delete(void* ptr) {
        cout << "Nạp chồng operator delete toàn cục" << endl;
        free(ptr);
    }

    int main() {
        int* p = new int(5);
        cout << *p << endl;
        delete p;
        return 0;
    }
    ```
    **Kết quả**:
    ```
    Nạp chồng operator new toàn cục, kích thước: 4
    5
    Nạp chồng operator delete toàn cục
    ```

- **Phiên bản nothrow**: Từ khóa `new` có một biến thể `nothrow` (`new (std::nothrow) Type`) trả về `nullptr` thay vì ném ngoại lệ nếu cấp phát thất bại.
- **Thực Tiễn Tốt Nhất**:
  - Sử dụng từ khóa `new` cho việc tạo và khởi tạo đối tượng tiêu chuẩn.
  - Chỉ nạp chồng `operator new` khi cần quản lý bộ nhớ cụ thể (ví dụ: bộ cấp phát tùy chỉnh).
  - Luôn kết hợp nạp chồng `operator new` với `operator delete` tương ứng để tránh rò rỉ bộ nhớ.
  - Cẩn thận khi sử dụng `operator new` độc lập, vì nó bỏ qua việc gọi hàm tạo và có thể dẫn đến đối tượng chưa được khởi tạo.

## Tóm Tắt
Từ khóa `new` trong C++ là một toán tử cấp cao cấp phát bộ nhớ và khởi tạo đối tượng bằng cách gọi hàm tạo, là cách chuẩn để tạo các đối tượng động. Ngược lại, `operator new` là một hàm cấp thấp chỉ cấp phát bộ nhớ thô, cung cấp sự linh hoạt cho việc quản lý bộ nhớ tùy chỉnh thông qua nạp chồng. Hiểu rõ vai trò và sự khác biệt của chúng là rất quan trọng để quản lý bộ nhớ hiệu quả trong các chương trình C++.