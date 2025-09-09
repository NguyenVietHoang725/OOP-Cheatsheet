# Công thức tổng quát để nạp chồng toán tử

Trong C++, nạp chồng toán tử cho phép bạn định nghĩa lại hành vi của các toán tử (như +, =, >>, <<, v.v.) khi chúng được sử dụng với các kiểu dữ liệu do người dùng định nghĩa (như class hoặc struct). Công thức tổng quát để nạp chồng toán tử là:

```cpp
return_type operator operator_symbol (parameters) {
    // Định nghĩa hành vi của toán tử
}
```

- `return_type`: Kiểu dữ liệu trả về của toán tử (có thể là void, kiểu dữ liệu của class, hoặc kiểu khác tùy thuộc vào toán tử).
- `operator`: Từ khóa để khai báo nạp chồng toán tử.
- `operator_symbol`: Ký hiệu của toán tử cần nạp chồng (ví dụ: +, =, >>, <<, v.v.).
- `parameters`: Các tham số mà toán tử sẽ nhận (tùy thuộc vào toán tử là đơn phân, nhị phân, hoặc khác).

---

# Các dạng nạp chồng toán tử

## 1. Nạp chồng trong class (phương thức thành viên):
Toán tử được định nghĩa như một hàm thành viên của class, trong đó đối tượng hiện tại (`this`) là toán hạng bên trái.
```cpp
class ClassName {
public:
    return_type operator operator_symbol (const ClassName& other) {
        // Thực hiện hành vi
    }
};
```

## 2. Nạp chồng ngoài class (hàm bạn - friend function hoặc hàm toàn cục):
Toán tử được định nghĩa bên ngoài class, thường yêu cầu cả hai toán hạng được truyền vào.
```cpp
return_type operator operator_symbol (const ClassName& left, const ClassName& right) {
    // Thực hiện hành vi
}
```

## 3. Toán tử đơn phân (như ++, --, -):
Chỉ cần một toán hạng, thường là đối tượng của class.
```cpp
return_type operator operator_symbol () {
    // Thực hiện hành vi
}
```

## 4. Toán tử đặc biệt (như >>, <<):
Thường được nạp chồng để làm việc với luồng nhập/xuất (std::istream, std::ostream).

```cpp
std::ostream& operator<< (std::ostream& os, const ClassName& obj) {
    // Xuất dữ liệu của obj
    return os;
}
```

---

# Ví dụ minh họa

## Ví dụ 1: Toán tử `+` (cộng hai đối tượng)

Giả sử ta có một class Vector2D biểu diễn vector 2 chiều, và ta muốn nạp chồng toán tử + để cộng hai vector.

```cpp
#include <iostream>
class Vector2D {
public:
    double x, y;

    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    // Nạp chồng toán tử + (phương thức thành viên)
    Vector2D operator+ (const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }
};

int main() {
    Vector2D v1(1.0, 2.0), v2(3.0, 4.0);
    Vector2D v3 = v1 + v2; // Sử dụng toán tử +
    std::cout << "v3 = (" << v3.x << ", " << v3.y << ")\n"; // Kết quả: v3 = (4, 6)
    return 0;
}
```

## Ví dụ 2: Toán tử `<<` và `>>` (xuất/nhập luồng)

Nạp chồng toán tử << để xuất và >> để nhập dữ liệu cho đối tượng Vector2D.

```cpp
#include <iostream>
class Vector2D {
public:
    double x, y;

    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    // Nạp chồng toán tử << (friend function)
    friend std::ostream& operator<< (std::ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;
    }

    // Nạp chồng toán tử >> (friend function)
    friend std::istream& operator>> (std::istream& is, Vector2D& v) {
        is >> v.x >> v.y;
        return is;
    }
};

int main() {
    Vector2D v;
    std::cout << "Nhap vector (x y): ";
    std::cin >> v; // Sử dụng toán tử >>
    std::cout << "Vector: " << v << "\n"; // Sử dụng toán tử <<
    return 0;
}
```

## Ví dụ 3: Toán tử `=` (gán)

Nạp chồng toán tử gán để sao chép dữ liệu giữa hai đối tượng Vector2D.

```cpp
#include <iostream>
class Vector2D {
public:
    double x, y;

    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    // Nạp chồng toán tử = (phương thức thành viên)
    Vector2D& operator= (const Vector2D& other) {
        if (this != &other) { // Tránh tự gán
            x = other.x;
            y = other.y;
        }
        return *this;
    }

    friend std::ostream& operator<< (std::ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;
    }
};

int main() {
    Vector2D v1(1.0, 2.0), v2;
    v2 = v1; // Sử dụng toán tử =
    std::cout << "v2 = " << v2 << "\n"; // Kết quả: v2 = (1, 2)
    return 0;
}
```

## Ví dụ 4: Toán tử `==` (so sánh bằng)

Nạp chồng toán tử == để kiểm tra xem hai vector có bằng nhau không.

```cpp
#include <iostream>
class Vector2D {
public:
    double x, y;

    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}

    // Nạp chồng toán tử == (phương thức thành viên)
    bool operator== (const Vector2D& other) const {
        return x == other.x && y == other.y;
    }

    friend std::ostream& operator<< (std::ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;
    }
};

int main() {
    Vector2D v1(1.0, 2.0), v2(1.0, 2.0), v3(3.0, 4.0);
    std::cout << "v1 == v2: " << (v1 == v2) << "\n"; // Kết quả: 1 (true)
    std::cout << "v1 == v3: " << (v1 == v3) << "\n"; // Kết quả: 0 (false)
    return 0;
}
```