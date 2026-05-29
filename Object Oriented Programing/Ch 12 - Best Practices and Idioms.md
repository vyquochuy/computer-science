# Chương 12: Các Thực hành Tốt nhất và Cơ chế Đặc thù (Best Practices and Idioms)

Chương này trình bày các thực hành tốt nhất (best practices) và cơ chế lập trình đặc thù (idioms) cốt lõi trong C++, giúp xây dựng mã nguồn an toàn hơn, dễ bảo trì hơn và tối ưu hiệu năng hơn. Việc làm chủ các quy tắc này là ranh giới phân biệt giữa lập trình viên C++ mới bắt đầu (novice C++) và lập trình viên C++ chuyên nghiệp (professional C++).

## RAII – Mọi Tài nguyên đều được đóng gói trong một Đối tượng

**RAII** (Resource Acquisition Is Initialization) là cơ chế đặc thù quan trọng nhất trong ngôn ngữ C++. Nó liên kết chặt chẽ vòng đời của tài nguyên với vòng đời của đối tượng. Tài nguyên được cấp phát trực tiếp trong hàm khởi tạo (constructor) và tự động giải phóng trong hàm hủy (destructor), bảo đảm an toàn thu hồi tài nguyên ngay cả khi chương trình ném ngoại lệ.

**Thực hành tốt – Ứng dụng các bộ chứa RAII**:

```cpp
// Cách làm tồi: tự quản lý tài nguyên thủ công
void bad() {
    int* data = new int[1000];
    // ... nếu có ngoại lệ xảy ra ở đây, bộ nhớ sẽ bị rò rỉ
    delete[] data;
}

// Cách làm tốt: ứng dụng bộ chứa RAII tiêu chuẩn
void good() {
    std::vector<int> data(1000);  // Bộ nhớ được cấp phát tự động trong hàm khởi tạo
    // ... tự động giải phóng trong hàm hủy khi biến data ra khỏi phạm vi hoạt động
}
```

**Ví dụ về lớp bao bọc RAII tự định nghĩa**:

```cpp
#include <cstdio>
#include <stdexcept>

class FileHandle {
    FILE* file;
public:
    FileHandle(const char* filename, const char* mode) {
        file = fopen(filename, mode);
        if (!file) throw std::runtime_error("cannot open file");
    }
    ~FileHandle() { if (file) fclose(file); }
    
    // Vô hiệu hóa sao chép, cho phép dịch chuyển (movable)
    FileHandle(const FileHandle&) = delete;
    FileHandle& operator=(const FileHandle&) = delete;
    FileHandle(FileHandle&& other) noexcept : file(other.file) { other.file = nullptr; }
    FileHandle& operator=(FileHandle&& other) noexcept {
        if (this != &other) {
            if (file) fclose(file);
            file = other.file;
            other.file = nullptr;
        }
        return *this;
    }
    
    FILE* get() const { return file; }
};
```

## Cơ chế sao chép và hoán đổi (Copy-and-Swap) cho Đảm bảo Ngoại lệ Mạnh

Cơ chế sao chép và hoán đổi (Copy-and-swap idiom) mang lại sự bảo đảm an toàn ngoại lệ mạnh mẽ (strong exception safety) cho các toán tử gán. Hơn nữa, nó hỗ trợ tự nhiên cả phép gán sao chép và gán dịch chuyển chỉ với một hàm duy nhất.

```cpp
#include <algorithm>

class String {
    char* data;
public:
    // ... các hàm khởi tạo, hàm hủy
    
    void swap(String& other) noexcept {
        std::swap(data, other.data);
    }
    
    // Một toán tử gán duy nhất xử lý hiệu quả cả sao chép và dịch chuyển
    String& operator=(String other) {  // Truyền bằng trị (pass by value)
        swap(other);                   // Hoán đổi không ném ngoại lệ
        return *this;
    }
};
```

Nếu chương trình cần sao chép, tham số `other` sẽ được tạo bằng hàm khởi tạo sao chép (có thể ném ngoại lệ). Nếu có thể dịch chuyển, nó sẽ được tạo bằng hàm khởi tạo dịch chuyển. Do thao tác hoán đổi (`swap`) được đánh dấu `noexcept`, trạng thái ban đầu của đối tượng gọi gán được đảm bảo giữ nguyên vẹn nếu có bất kỳ ngoại lệ nào phát sinh trước khi quá trình hoán đổi thực sự diễn ra.

## Cơ chế Pimpl (Pointer to Implementation Idiom)

Cơ chế Pimpl (Con trỏ tới phần triển khai) ẩn toàn bộ chi tiết triển khai cụ thể của một lớp vào trong một lớp phụ trợ riêng biệt, giúp giảm thiểu sự phụ thuộc lẫn nhau khi biên dịch, tối ưu thời gian build ứng dụng, và duy trì tính tương thích nhị phân tốt hơn.

**Tệp tiêu đề (Widget.h)**:

```cpp
// Widget.h
#pragma once
#include <memory>

class Widget {
public:
    Widget();
    ~Widget();
    void doSomething();
    
private:
    struct Impl;
    std::unique_ptr<Impl> pImpl;
};
```

**Tệp triển khai chi tiết (Widget.cpp)**:

```cpp
// Widget.cpp
#include "Widget.h"
#include <iostream>

struct Widget::Impl {
    int internalData = 42;
    void helper() {
        std::cout << "Helper called\n";
    }
};

Widget::Widget() : pImpl(std::make_make_unique<Impl>()) {}
Widget::~Widget() = default; // Yêu cầu định nghĩa tại đây vì Impl là kiểu chưa hoàn chỉnh (incomplete) trong tệp tiêu đề

void Widget::doSomething() {
    pImpl->helper();
}
```

Lợi ích vượt trội của Pimpl:
- Thay đổi chi tiết triển khai bên trong mà không cần phải biên dịch lại mã nguồn phía khách hàng (client code).
- Ẩn hoàn toàn các hàm thành viên sinh ra tự động bởi trình biên dịch.
- Giảm thiểu tối đa việc bao hàm các tệp tiêu đề phức tạp (`#include`).

## Cơ chế Giao diện phi ảo (NVI - Non-virtual Interface Idiom)

Cơ chế GVI khuyên rằng các giao diện công khai (public interfaces) nên được thiết kế dưới dạng phi ảo (non-virtual), và chúng sẽ gọi các hàm ảo private hoặc protected bên trong. Triết lý này áp dụng mô hình mẫu phương thức (template method pattern) và hỗ trợ tích hợp các bước kiểm tra tiền điều kiện/hậu điều kiện tại một nơi tập trung duy nhất.

```cpp
class Base {
public:
    // Giao diện phi ảo công khai
    void execute() {
        // Tiền xử lý (ví dụ: ghi nhật ký, khóa mutex)
        doExecute();
        // Hậu xử lý
    }
    
private:
    virtual void doExecute() = 0;  // Các lớp dẫn xuất sẽ nạp chồng hàm này
};

class Derived : public Base {
private:
    void doExecute() override {
        // Phần triển khai thực tế
    }
};
```

Lợi ích của NVI:
- Kiểm soát chặt chẽ các bất biến (invariants) trước và sau khi thực hiện thao tác.
- Cho phép thực hiện các cập nhật thay đổi ở lớp cơ sở mà không làm ảnh hưởng đến các lớp dẫn xuất.

## Mẫu khuôn mẫu lặp lại kỳ lạ (CRTP - Curiously Recurring Template Pattern)

CRTP hiện thực hóa cơ chế đa hình tĩnh (static polymorphism) (điều phối tại thời điểm biên dịch - compile-time dispatch) giúp loại bỏ hoàn toàn các chi phí phát sinh của bảng phương thức ảo (vtable). Một lớp dẫn xuất sẽ kế thừa từ một khuôn mẫu lớp cơ sở, đồng thời truyền chính nó làm đối số khuôn mẫu.

```cpp
#include <iostream>

template <typename Derived>
class Shape {
public:
    void draw() const {
        static_cast<const Derived*>(this)->drawImpl();
    }
};

class Circle : public Shape<Circle> {
public:
    void drawImpl() const {
        std::cout << "Drawing a circle\n";
    }
};

class Square : public Shape<Square> {
public:
    void drawImpl() const {
        std::cout << "Drawing a square\n";
    }
};

template <typename T>
void render(const Shape<T>& shape) {
    shape.draw();  // Điều phối tại thời điểm biên dịch (compile-time dispatch)
}
```

Các trường hợp ứng dụng phổ biến:
- Lớp Mixin (bổ sung chức năng chung cho nhiều lớp).
- Đếm tổng số lượng thực thể đang hoạt động của một lớp cụ thể.
- Khuôn mẫu biểu thức (expression templates) (như thư viện Eigen, Boost.Proto).

## Quy tắc không thành phần (Rule of Zero)

Quy tắc không thành phần (Rule of Zero) khuyên rằng bạn nên tránh tự viết các hàm hủy tùy biến, hàm khởi tạo sao chép/dịch chuyển, và các toán tử gán. Thay vào đó, hãy sử dụng các bộ chứa RAII tiêu chuẩn và các con trỏ thông minh. Các hàm đặc biệt sinh ra mặc định bởi trình biên dịch sẽ luôn hoạt động chính xác nếu tất cả các thuộc tính thành viên bên trong bản thân chúng đều là các kiểu RAII.

```cpp
#include <string>
#include <vector>

// Tốt – Tuân thủ hoàn toàn Quy tắc không thành phần
class Person {
    std::string name;
    std::vector<int> scores;
public:
    // Không cần tự viết hàm hủy, sao chép/dịch chuyển, hoặc phép gán
    Person(const std::string& n) : name(n) {}
    void addScore(int s) { scores.push_back(s); }
};
```

Nếu bắt buộc phải tự quản lý tài nguyên, hãy đóng gói nó vào trong một lớp RAII chuyên biệt riêng biệt. Từ đó, lớp sở hữu sẽ luôn giữ được sự đơn giản và sạch sẽ.

## Sử dụng hằng số đúng cách (Const Correctness)

Hãy sử dụng từ khóa `const` ở mọi nơi có thể: trên biến số, tham số truyền vào, hàm thành viên, và kiểu dữ liệu trả về. Điều này giúp ngăn chặn các sửa đổi ngoài ý muốn và tăng tính tường minh của mã nguồn.

```cpp
#include <vector>

class DataProcessor {
    std::vector<int> data;
public:
    void add(int x) { data.push_back(x); }  // Hàm thành viên phi const
    
    int getSum() const {  // Hàm thành viên const – không chỉnh sửa đối tượng
        int sum = 0;
        for (int v : data) sum += v;
        return sum;
    }
    
    const std::vector<int>& getData() const { return data; }  // Trả về tham chiếu hằng
};

void process(const DataProcessor& dp) {
    int sum = dp.getSum();  // Hợp lệ
    // dp.add(5);  // Lỗi: không thể gọi hàm phi const trên một tham chiếu hằng
}
```

Các nguyên tắc hướng dẫn:
- Khai báo các hàm thành viên là `const` nếu chúng không làm biến đổi trạng thái logic (logical state) của đối tượng.
- Ưu tiên truyền tham số bằng tham chiếu hằng `const&` trừ khi bạn thực sự cần tạo một bản sao hoặc cần chỉnh sửa nó trực tiếp.
- Sử dụng bộ duyệt hằng (`cbegin()`, `cend()`) khi chỉ cần duyệt đọc dữ liệu.

## Ưu tiên sử dụng `nullptr` thay vì `NULL` hoặc `0`

`nullptr` là một hằng con trỏ thuộc kiểu `std::nullptr_t`, có khả năng chuyển đổi tự động sang bất kỳ kiểu con trỏ nào nhưng không thể chuyển đổi sang kiểu số nguyên. Nó giúp loại bỏ hoàn toàn các trường hợp mơ hồ và các lỗi phát sinh khi phân giải quá tải hàm.

```cpp
#include <iostream>

void f(int) { std::cout << "int\n"; }
void f(char*) { std::cout << "char*\n"; }

int main() {
    f(0);       // Gọi f(int)
    f(NULL);    // Có thể gọi f(int) nếu NULL được định nghĩa là 0
    f(nullptr); // Gọi f(char*) – Hoàn toàn chính xác
}
```

## Sử dụng `enum class` thay thế cho `enum` thông thường

Lớp liệt kê có phạm vi (scoped enumerations) (C++11) mang lại tính an toàn kiểu cao, phạm vi định danh rõ ràng, và hỗ trợ khai báo trước (forward declaration) tốt hơn.

| Đặc tính | `enum` thông thường (unscoped) | `enum class` có phạm vi (scoped) |
|---|---|---|
| **Chuyển đổi ngầm định sang int** | Có | Không |
| **Phạm vi định danh** | Bị tràn ra phạm vi chứa bên ngoài | Bắt buộc phải sử dụng tiền tố `EnumName::` |
| **Kiểu dữ liệu nền tảng** | Phụ thuộc trình biên dịch quyết định | Mặc định là `int`, có thể chỉ định cụ thể |
| **Khai báo trước** | Chỉ khi chỉ định rõ kiểu nền | Được hỗ trợ mặc định |

```cpp
// Cách làm cũ: enum unscoped
enum Color { Red, Green, Blue };
int c = Red;  // Chuyển đổi ngầm định sang int – Dễ gây lỗi logic

// Cách làm mới: enum class scoped
enum class Color { Red, Green, Blue };
Color c = Color::Red;
// int i = Color::Red;  // Lỗi: không cho phép chuyển đổi ngầm định sang int
```

## Tránh sử dụng từ khóa `new`/`delete` thô – Ưu tiên con trỏ thông minh và bộ chứa

Mã nguồn C++ hiện đại rất hiếm khi sử dụng trực tiếp các từ khóa `new` và `delete` thô. Hãy ưu tiên lựa chọn:

- **Cấp phát tự động (Automatic storage)** (cấp phát trên vùng nhớ Stack) – tốc độ nhanh nhất và an toàn nhất.
- **Bộ chứa tiêu chuẩn** (`std::vector`, `std::string`) thay cho các mảng thô.
- **Con trỏ thông minh** (`std::unique_ptr`, `std::shared_ptr`) để quản lý các đối tượng cấp phát động.

```cpp
// Tránh cách viết này
int* p = new int(42);
delete p;

// Nên viết như thế này
auto p = std::make_unique<int>(42);  // unique_ptr tự động quản lý bộ nhớ

// Tránh cách viết này
int* arr = new int[100];
delete[] arr;

// Nên viết như thế này
std::vector<int> arr(100);
```

## Sử dụng chỉ định `override` và `final`

- `override` – khai báo tường minh rằng một phương thức ảo nạp chồng lại một phương thức từ lớp cơ sở. Trình biên dịch sẽ kiểm tra sự tương thích của chữ ký hàm, giúp phát hiện lỗi sai sớm ngay khi dịch.
- `final` – ngăn chặn các lớp con tiếp tục nạp chồng một phương thức ảo hoặc ngăn chặn một lớp bị kế thừa.

```cpp
class Base {
public:
    virtual void foo(int);
    virtual void bar() final;
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void foo(int) override;   // Hợp lệ
    // void foo(double) override; // Lỗi: chữ ký hàm không khớp với phương thức ảo lớp cơ sở
    // void bar() override;   // Lỗi: phương thức bar đã được đánh dấu final ở lớp cơ sở
};

class FinalClass final : public Base {
    // Lớp này đã được đánh dấu final, không thể bị kế thừa tiếp
};
```

Ứng dụng `override` là quy chuẩn bắt buộc để duy trì hệ thống phân cấp đa hình an toàn, giúp biến các lỗi logic tiềm ẩn thành các lỗi biên dịch rõ ràng.

## Bảng tổng hợp thực hành tốt nhất

| Thực hành tốt | Lợi ích cốt lõi mang lại | Ví dụ minh họa |
|---|---|---|
| **RAII** | Tự động dọn dẹp tài nguyên | `std::vector<int> v(100);` |
| **Sao chép và hoán đổi** | Đảm bảo an toàn ngoại lệ mạnh | `String& operator=(String other) { swap(other); return *this; }` |
| **Pimpl** | Giảm thiểu phụ thuộc biên dịch | `std::unique_ptr<Impl> pImpl;` |
| **NVI** | Kiểm soát luồng tiền/hậu điều kiện | Giao diện công khai phi ảo gọi hàm ảo private |
| **CRTP** | Đa hình tĩnh hiệu năng cao | `class Derived : public Base<Derived>` |
| **Quy tắc không thành phần** | Đơn giản hóa, hạn chế lỗi logic | Chỉ sử dụng các thuộc tính thành viên RAII tự quản lý |
| **Const correctness** | Ngăn chặn chỉnh sửa ngoài ý muốn | `int get() const { return x; }` |
| **`nullptr`** | Tránh lỗi mơ hồ con trỏ hằng | `int* p = nullptr;` |
| **`enum class`** | An toàn kiểu cao, định danh rõ ràng | `enum class Color { Red };` |
| **Con trỏ thông minh** | Quản lý quyền sở hữu bộ nhớ | `auto p = std::make_unique<T>();` |
| **`override`** | Phát hiện sớm lỗi sai chữ ký hàm | `void foo() override;` |
| **`final`** | Đóng băng thiết kế đa hình/kế thừa | `class C final { };` |

Áp dụng một cách nhất quán các thực hành tốt nhất này sẽ nâng tầm đáng kể chất lượng, độ an toàn và tính dễ đọc của mã nguồn C++ của bạn. Chúng không chỉ đơn thuần là những khuyến nghị về phong cách viết mã (style guide) – mà là những công cụ thiết yếu để xây dựng các hệ thống C++ chuyên nghiệp.
