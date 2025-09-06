# Từ Khóa `const` Trong C++

Trong C++, từ khóa `const` được sử dụng để khai báo các biến, con trỏ, hàm thành viên, tham số hàm, và kiểu trả về là hằng số, ngăn chặn việc sửa đổi sau khi khởi tạo. Điều này thúc đẩy tính bất biến, tăng cường an toàn mã, và hỗ trợ đóng gói trong lập trình hướng đối tượng. Bài viết này giải thích các cách sử dụng của từ khóa `const` với các ví dụ minh họa.

## Các Cách Sử Dụng Từ Khóa `const`

### 1. Biến Hằng
- **Định nghĩa**: Một biến `const` không thể được sửa đổi sau khi khởi tạo và phải được khởi tạo ngay khi khai báo.
- **Quy tắc**:
  - Phải được khởi tạo tại thời điểm khai báo.
  - Không thể gán giá trị mới sau khi khởi tạo.
  - Bất kỳ nỗ lực nào để sửa đổi biến `const` sẽ dẫn đến lỗi biên dịch (CTE).
- **Cú pháp**:
  ```cpp
  const data_type variable_name = value;
  ```
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // const int x; // CTE: biến const chưa khởi tạo
      // x = 9; // CTE: không thể gán cho biến const
      const int y = 10;
      // y = 20; // CTE: không thể sửa đổi biến const
      cout << y; // Kết quả: 10
      return 0;
  }
  ```

### 2. `const` với Con Trỏ
Từ khóa `const` có thể được sử dụng với con trỏ theo ba cách khác nhau, ảnh hưởng đến con trỏ, dữ liệu mà nó trỏ tới, hoặc cả hai.

#### a. Con Trỏ Trỏ Đến Giá Trị Hằng
- **Cú pháp**: `const data_type* var_name;`
- **Mô tả**: Dữ liệu mà con trỏ trỏ tới là hằng số và không thể sửa đổi, nhưng con trỏ có thể trỏ đến một địa chỉ khác.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 10;
      char y = 'M';
      const int* i = &x;
      const char* j = &y;
      x = 9; // Được phép: x không phải const
      y = 'A'; // Được phép: y không phải const
      // *i = 6; // CTE: không thể sửa đổi int const
      // *j = 'B'; // CTE: không thể sửa đổi char const
      cout << *i << " " << *j; // Kết quả: 9 A
      return 0;
  }
  ```

#### b. Con Trỏ Hằng Trỏ Đến Giá Trị Không Hằng
- **Cú pháp**: `data_type* const var_name;`
- **Mô tả**: Con trỏ là hằng số và không thể trỏ đến địa chỉ khác, nhưng dữ liệu mà nó trỏ tới có thể sửa đổi.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 5, z = 6;
      char y = 'A', p = 'C';
      int* const i = &x;
      char* const j = &y;
      *i = 10; // Được phép: dữ liệu có thể sửa đổi
      *j = 'D'; // Được phép: dữ liệu có thể sửa đổi
      // i = &z; // CTE: không thể thay đổi con trỏ const
      // j = &p; // CTE: không thể thay đổi con trỏ const
      cout << *i << " và " << *j << endl; // Kết quả: 10 và D
      return 0;
  }
  ```

#### c. Con Trỏ Hằng Trỏ Đến Giá Trị Hằng
- **Cú pháp**: `const data_type* const var_name;`
- **Mô tả**: Cả con trỏ và dữ liệu mà nó trỏ tới đều là hằng số, không thể sửa đổi.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 9;
      const int* const i = &x;
      // *i = 10; // CTE: không thể sửa đổi dữ liệu const
      // i = nullptr; // CTE: không thể thay đổi con trỏ const
      char y = 'A';
      const char* const j = &y;
      // *j = 'B'; // CTE: không thể sửa đổi dữ liệu const
      // j = nullptr; // CTE: không thể thay đổi con trỏ const
      cout << *i << " và " << *j; // Kết quả: 9 và A
      return 0;
  }
  ```

#### d. Truyền Con Trỏ vào Hàm
- Truyền một con trỏ `const` vào hàm mong đợi tham số con trỏ không const sẽ gây ra CTE, vì vi phạm tính đúng đắn của const.
- **Ví dụ** (Trường hợp lỗi):
  ```cpp
  #include <iostream>
  using namespace std;

  int foo(int* y) { return *y; }

  int main() {
      int z = 8;
      const int* x = &z;
      // cout << foo(x); // CTE: không thể chuyển const int* thành int*
      return 0;
  }
  ```
- **Ví dụ** (Trường hợp đúng):
  ```cpp
  #include <iostream>
  using namespace std;

  void printfunc(int* ptr) {
      cout << "Giá trị: " << *ptr << endl;
      cout << "Địa chỉ của ptr: " << &ptr << endl;
  }

  int main() {
      int x = 10;
      int* const i = &x;
      printfunc(i); // Được phép: tạo con trỏ mới
      cout << "Địa chỉ của i: " << &i << endl;
      return 0;
  }
  ```
  **Kết quả**:
  ```
  Giá trị: 10
  Địa chỉ của ptr: [địa chỉ nào đó]
  Địa chỉ của i: [địa chỉ khác]
  ```

- **Tóm tắt về con trỏ**:
  - `const int* ptr`: Con trỏ trỏ đến giá trị const (dữ liệu bất biến).
  - `int* const ptr`: Con trỏ const trỏ đến giá trị không const (con trỏ bất biến).
  - `const int* const ptr`: Con trỏ const trỏ đến giá trị const (cả hai đều bất biến).

### 3. Hàm Thành Viên Hằng
- **Định nghĩa**: Một hàm thành viên được khai báo với `const` đảm bảo không sửa đổi trạng thái của đối tượng (trừ các thành viên `mutable`).
- **Cú pháp**:
  ```cpp
  return_type function_name() const;
  ```
- **Mô tả**:
  - Có thể được gọi trên cả đối tượng const và không const.
  - Đảm bảo hàm không sửa đổi các thành viên không phải `mutable`.
  - Đối tượng const chỉ có thể gọi các hàm thành viên const.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  class Test {
      int value;
  public:
      Test(int v = 0) : value(v) {}
      int getValue() const { // Hàm thành viên const
          // value = 100; // CTE: không thể sửa đổi đối tượng
          return value;
      }
      void setValue(int val) { // Hàm thành viên không const
          value = val;
      }
  };

  int main() {
      Test t(20);
      cout << t.getValue() << endl; // Kết quả: 20
      const Test t_const(10);
      cout << t_const.getValue() << endl; // Kết quả: 10
      // t_const.setValue(15); // CTE: đối tượng const không thể gọi hàm không const
      t.setValue(12);
      cout << t.getValue() << endl; // Kết quả: 12
      return 0;
  }
  ```

### 4. Tham Số Hàm và Kiểu Trả Về Hằng
- **Tham số hằng**:
  - Khai báo tham số là `const` ngăn chặn việc sửa đổi trong hàm.
  - Truyền giá trị const vào tham số không const sẽ gây ra CTE khi truyền bằng tham chiếu.
- **Ví dụ**:
  ```cpp
  #include <iostream>
  using namespace std;

  void foo(const int y) {
      // y = 6; // CTE: không thể sửa đổi tham số const
      cout << y;
  }

  void foo1(int y) {
      y = 5;
      cout << y;
  }

  int main() {
      int x = 9;
      const int z = 10;
      foo(z); // Kết quả: 10
      cout << endl;
      foo1(x); // Kết quả: 5
      return 0;
  }
  ```

- **Kiểu trả về hằng**:
  - Kiểu trả về `const` đảm bảo giá trị trả về không thể bị sửa đổi.
  - **Ví dụ**:
    ```cpp
    #include <iostream>
    using namespace std;

    const int foo(int y) {
        y--;
        return y;
    }

    int main() {
        int x = 9;
        const int z = 10;
        cout << foo(x) << '\n' << foo(z); // Kết quả: 8 9
        return 0;
    }
    ```

- **Tham số và kiểu trả về hằng**:
  - Cả tham số và kiểu trả về đều là const, ngăn chặn sửa đổi.
  - **Ví dụ**:
    ```cpp
    #include <iostream>
    using namespace std;

    const int foo(const int y) {
        // y = 9; // CTE: không thể sửa đổi tham số const
        return y;
    }

    int main() {
        int x = 9;
        const int z = 10;
        cout << foo(x) << '\n' << foo(z); // Kết quả: 9 10
        return 0;
    }
    ```

## Ghi Chú Quan Trọng
- **Khởi tạo**: Biến và đối tượng const phải được khởi tạo tại thời điểm khai báo, thường sử dụng hàm tạo cho đối tượng.
- **Tính đúng đắn của const**: Đảm bảo tính bất biến, giảm lỗi và cải thiện tính rõ ràng của mã.
- **Trường hợp lỗi**:
  - Sửa đổi biến hoặc đối tượng const dẫn đến CTE.
  - Truyền giá trị const vào tham số không const bằng tham chiếu gây ra CTE.
  - Hàm thành viên không const không thể được gọi trên đối tượng const.
- **Thực Tiễn Tốt Nhất**:
  - Sử dụng `const` cho các biến, con trỏ, và tham số không nên thay đổi.
  - Khai báo các hàm thành viên là `const` khi chúng không sửa đổi đối tượng.
  - Sử dụng kiểu trả về `const` để ngăn chặn sửa đổi giá trị trả về ngoài ý muốn.

## Tóm Tắt
Từ khóa `const` trong C++ là một công cụ mạnh mẽ để thực thi tính bất biến cho các biến, con trỏ, hàm thành viên, tham số, và kiểu trả về. Bằng cách ngăn chặn các sửa đổi ngoài ý muốn, nó tăng cường an toàn mã và hỗ trợ đóng gói. Hiểu rõ các ứng dụng của nó là cần thiết để viết các chương trình C++ mạnh mẽ và dễ bảo trì.