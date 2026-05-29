# LẬP TRÌNH HƯỚNG ĐỐI TƯỢNG TRONG C++ (OBJECT ORIENTED PROGRAMMING IN C++)

## Chương 1: Giới thiệu về OOP và Các kiến thức C++ cơ bản (Introduction to OOP and C++ Basics)

* Lập trình hướng thủ tục so với Lập trình hướng đối tượng (Procedural vs Object-Oriented Programming)
* Lợi ích của OOP: tính mô-đun (modularity), tính tái sử dụng (reusability), khả năng bảo trì (maintainability)
* Các khái niệm cốt lõi của OOP (Core OOP concepts):
  * Tính đóng gói (Encapsulation)
  * Tính trừu tượng (Abstraction)
  * Tính kế thừa (Inheritance)
  * Tính đa hình (Polymorphism)
* C++ dưới vai trò ngôn ngữ đa mô hình (multi‑paradigm language - hướng thủ tục, hướng đối tượng, tổng quát, chức năng)
* Ôn tập cú pháp C++ cơ bản:
  * Nhập/xuất dữ liệu (`cin`, `cout`, `cerr`)
  * Không gian tên (Namespaces - `std` và không gian tên tự định nghĩa)
  * Tham chiếu so với con trỏ (References vs pointers)
  * Bộ nhớ động: `new`, `delete`, `new[]`, `delete[]`
  * Quá tải hàm (Function overloading) và tham số mặc định (default arguments)

---

## Chương 2: Lớp và Đối tượng (Classes and Objects)

* Định nghĩa một lớp: các phạm vi truy cập (access specifiers - `public`, `private`, `protected`)
* Thuộc tính thành viên (data members) và phương thức thành viên (member functions)
* Tạo đối tượng: cấp phát trên vùng nhớ Stack so với vùng nhớ Heap
* Truy cập các thành viên của lớp: toán tử chấm (`.`) và toán tử mũi tên (`->`)
* **Con trỏ `this`** – tham chiếu ngầm định đến đối tượng hiện tại
* Các thành viên tĩnh (Static members):
  * Thuộc tính tĩnh (Static data members - chia sẻ chung giữa tất cả các đối tượng của lớp)
  * Phương thức tĩnh (Static member functions - chỉ truy cập được các thành viên tĩnh khác)

### Hàm khởi tạo và Hàm hủy (Constructors and Destructor)

* Hàm khởi tạo mặc định (Default constructor)
* Hàm khởi tạo có tham số (Parameterized constructor)
* Quá tải hàm khởi tạo (Constructor overloading)
* Hàm khởi tạo với tham số mặc định (Constructor with default arguments)
* Hàm khởi tạo sao chép (Copy constructor - sao chép nông so với sao chép sâu / shallow vs deep copy)
* **Danh sách khởi tạo thành viên (Member initializer list)** – quy chuẩn tốt nhất để khởi tạo thuộc tính
* Hàm hủy (Destructor): vai trò và thứ tự thực thi hủy bỏ đối tượng
* Từ khóa `explicit` dùng để ngăn chặn việc chuyển đổi kiểu dữ liệu ngầm định

---

## Chương 3: Tính đóng gói và Ẩn giấu dữ liệu (Encapsulation and Data Hiding)

* Tại sao tính đóng gói lại quan trọng – cơ chế bảo vệ trạng thái nội bộ của đối tượng
* Phương thức Getter (truy cập - accessors) và Setter (thay đổi - mutators)
* Tính đúng đắn của hằng số (Const correctness):
  * Đối tượng hằng (Const objects) và phương thức hằng (const member functions)
  * Thuộc tính hằng (Const member variables - bắt buộc khởi tạo qua danh sách khởi tạo thành viên)
* Hàm bạn (Friend functions) và Lớp bạn (Friend classes):
  * Khi nào và tại sao cần sử dụng từ khóa `friend`
  * Đánh đổi hiệu năng và thiết kế: phá vỡ tính đóng gói so với sự tiện lợi khi lập trình
* Từ khóa `mutable` – cho phép sửa đổi thuộc tính thành viên ngay bên trong một đối tượng hằng

---

## Chương 4: Tính kế thừa (Inheritance)

* Cú pháp định nghĩa lớp cơ sở (lớp cha - base class) và lớp dẫn xuất (lớp con - derived class)
* Phạm vi truy cập trong kế thừa: `public`, `protected`, `private`
  * Ảnh hưởng của chúng đối với khả năng truy cập các thành viên ở lớp con
* **Các loại kế thừa (Types of inheritance)**:
  * Đơn kế thừa (Single inheritance)
  * Đa kế thừa (Multiple inheritance)
  * Kế thừa nhiều cấp (Multilevel inheritance)
  * Kế thừa phân cấp (Hierarchical inheritance)
  * Kế thừa lai (Hybrid inheritance - dẫn đến bài toán kim cương diamond problem)

### Thứ tự gọi Hàm khởi tạo/Hàm hủy

* Quy trình khởi tạo: lớp cha (base) $\to$ lớp con (derived)
* Quy trình hủy: lớp con (derived) $\to$ lớp cha (base)
* Cách truyền tham số lên hàm khởi tạo của lớp cha

### Bài toán Kim cương và Kế thừa ảo (Diamond Problem and Virtual Inheritance)

* Lớp cơ sở ảo (Virtual base classes)
* Cơ chế kế thừa ảo giải quyết sự mơ hồ nhập nhằng (ambiguity) như thế nào
* Xem xét về cấu trúc tổ chức bộ nhớ (Memory layout)

### Khai báo `using` trong lớp con

* Phục hồi quyền truy cập đến các thành viên của lớp cha

---

## Chương 5: Tính đa hình (Polymorphism)

### Đa hình lúc biên dịch (Compile‑Time Polymorphism - Đa hình tĩnh)

* Quá tải hàm (Function overloading)
* Quá tải toán tử (Operator overloading - xem chi tiết tại Chương 6)

### Đa hình lúc thực thi (Run‑Time Polymorphism - Đa hình động)

* **Hàm ảo (Virtual functions)** – cơ chế hoạt động và bảng phương thức ảo (vtable)
* Định nghĩa đè (Overriding) so với Ẩn giấu phương thức (Hiding)
* Từ khóa đặc tả `override` (C++11)
* Từ khóa đặc tả `final` (ngăn chặn việc định nghĩa đè hoặc kế thừa tiếp theo)

### Lớp trừu tượng và Giao diện (Abstract Classes and Interfaces)

* Hàm ảo thuần túy (Pure virtual functions - `= 0`)
* Lớp trừu tượng (Abstract class) – lớp không thể khởi tạo đối tượng trực tiếp
* Giả lập giao diện (Interface emulation) trong C++ (lớp chỉ chứa duy nhất các hàm ảo thuần túy)

### Hàm hủy ảo (Virtual Destructors)

* Tại sao hàm hủy của lớp cha bắt buộc phải là hàm hủy ảo
* Hành vi không xác định (Undefined behaviour) khi giải phóng đối tượng lớp con thông qua con trỏ lớp cha mà không có hàm hủy ảo

### Thông tin kiểu dữ liệu lúc thực thi (RTTI - Run‑Time Type Information)

* Toán tử `typeid` và lớp `type_info`
* Phép ép kiểu `dynamic_cast` (ép kiểu xuống một cách an toàn - safe downcasting)
* Khi nào RTTI là cần thiết – và khi nào nên tránh sử dụng

---

## Chương 6: Quá tải toán tử (Operator Overloading)

* Các quy tắc và giới hạn của việc quá tải toán tử
* Quá tải toán tử dưới dạng phương thức thành viên so với hàm tự do bên ngoài (hàm bạn friend)

### Toán tử một ngôi (Unary Operators)

* Toán tử `++` (tiền tố prefix và hậu tố postfix)
* Toán tử `--`
* Toán tử `!`, `~`, `-` (toán tử trừ một ngôi)

### Toán tử hai ngôi (Binary Operators)

* Toán tử số học: `+`, `-`, `*`, `/`, `%`
* Toán tử quan hệ: `==`, `!=`, `<`, `>`, `<=`, `>=`
* Toán tử gán: `=` (sao chép sâu, cơ chế copy‑and‑swap)
* Toán tử gán phức hợp: `+=`, `-=`, v.v.
* Toán tử chỉ số `[]` (phiên bản hằng const và không hằng)
* Toán tử gọi hàm `()` – đối tượng hàm (functors)
* Toán tử chèn luồng `<<` và trích xuất luồng `>>` (bắt buộc phải khai báo dưới dạng hàm tự do bên ngoài lớp)

### Quá tải toán tử `new` và `delete`

* Tự tùy biến cơ chế quản lý bộ nhớ cho một lớp học cụ thể

---

## Chương 7: Xử lý ngoại lệ (Exception Handling)

* Phương pháp xử lý lỗi truyền thống so với Xử lý ngoại lệ
* Các từ khóa ngoại lệ trong C++: `try`, `catch`, `throw`
* Phát sinh ngoại lệ (throw) từ hàm khởi tạo và hàm hủy
* Bắt ngoại lệ (catch) bằng tham trị, tham chiếu, hoặc con trỏ – quy chuẩn tốt nhất (bắt bằng tham chiếu hằng / catch by const reference)
* Cấu trúc phân cấp ngoại lệ chuẩn (`<stdexcept>`):
  * `std::exception`, `std::logic_error`, `std::runtime_error`
  * `std::bad_alloc`, `std::bad_cast`, v.v.
* Ràng buộc ngoại lệ (đã lỗi thời) so với từ khóa đặc tả `noexcept`
* Quy trình tháo gỡ ngăn xếp (Stack unwinding) và quản lý tài nguyên
* Cơ chế RAII (Resource Acquisition Is Initialization) – chìa khóa vàng cho mã nguồn an toàn ngoại lệ (exception‑safe)

---

## Chương 8: Khuôn mẫu (Lập trình tổng quát) (Templates - Generic Programming)

### Khuôn mẫu hàm (Function Templates)

* Cú pháp và cơ chế tự động suy diễn kiểu dữ liệu (type deduction)
* Cơ chế khởi tạo thực thể khuôn mẫu (Template instantiation)
* Quá tải khuôn mẫu hàm (Overloading function templates)

### Khuôn mẫu lớp (Class Templates)

* Định nghĩa lớp tổng quát (ví dụ: `Stack<T>`)
* Phương thức thành viên định nghĩa bên trong hoặc bên ngoài khuôn mẫu lớp
* Chuyên biệt hóa khuôn mẫu (Template specialization - chuyên biệt hóa hoàn toàn hoặc một phần)
* Các tham số khuôn mẫu không phải kiểu dữ liệu (Non‑type template parameters - ví dụ: `array<T, N>`)

### Khuôn mẫu số lượng tham số biến đổi (Variadic Templates) (C++11)

* Toán tử `sizeof...`
* Cơ chế mở rộng đệ quy và biểu thức gập (fold expressions) (C++17)

### Siêu lập trình khuôn mẫu (Template Metaprogramming - các khái niệm cơ bản)

* Thực hiện tính toán ngay trong quá trình biên dịch (Compile‑time computations)
* Từ khóa `static_assert` phục vụ kiểm tra điều kiện lúc biên dịch

---

## Chương 9: Thư viện Khuôn mẫu Chuẩn (STL - Standard Template Library)

### Tổng quan

* Các thành phần: Bộ lưu trữ (Containers), Bộ duyệt (Iterators), Thuật toán (Algorithms), Hàm đối tượng (Functors), Bộ cấp phát (Allocators)
* Cam kết về độ phức tạp thuật toán thời gian của STL

### Bộ lưu trữ (Containers)

| Danh mục | Ví dụ điển hình |
|----------------|-----------------------------------------------|
| Dãy tuần tự (Sequence) | `vector`, `deque`, `list`, `forward_list`, `array` |
| Liên kết (Associative) | `set`, `multiset`, `map`, `multimap` |
| Không sắp thứ tự (Unordered) | `unordered_set`, `unordered_map`, v.v. |
| Bộ chuyển đổi (Container adaptors) | `stack`, `queue`, `priority_queue` |

### Bộ duyệt (Iterators)

* Bộ duyệt đầu vào (input), đầu ra (output), xuôi (forward), hai chiều (bidirectional), truy cập ngẫu nhiên (random‑access)
* Các quy tắc vô hiệu hóa bộ duyệt (Iterator invalidation rules - đặc biệt đối với `vector`)
* Bộ duyệt ngược (`rbegin`, `rend`)

### Các thuật toán thông dụng (`<algorithm>`)

* Thuật toán sắp xếp, tìm kiếm, phân hoạch, và các thao tác trên đống heap
* Tìm giá trị min/max, sinh hoán vị, các thuật toán số học (`<numeric>`)

### Hàm đối tượng (Functors / Function Objects)

* Các hàm đối tượng định nghĩa sẵn (`std::plus`, `std::greater`, v.v.)
* Biểu thức Lambda (C++11) – cú pháp, danh sách bắt giữ (capture lists), lambda có thể thay đổi (mutable lambdas)
* Lớp `std::function` – cơ chế xóa kiểu dữ liệu (type erasure) cho các đối tượng có khả năng gọi được (callables)

### Con trỏ thông minh (Smart Pointers) (C++11)

* Con trỏ sở hữu độc quyền `std::unique_ptr`
* Con trỏ đếm tham chiếu `std::shared_ptr`
* Con trỏ liên kết yếu tránh chu trình `std::weak_ptr`
* Các hàm tạo an toàn: `std::make_unique` / `std::make_shared`

---

## Chương 10: Các tính năng C++ nâng cao (Advanced C++ Features)

### Cơ chế dịch chuyển và Chuyển tiếp hoàn hảo (Move Semantics and Perfect Forwarding)

* Tham chiếu lvalue và tham chiếu rvalue (`&&`)
* Hàm khởi tạo dịch chuyển (Move constructor) và toán tử gán dịch chuyển (move assignment operator)
* Quy tắc năm thành phần (Rule of Five - mở rộng từ Quy tắc ba thành phần kèm theo các thao tác dịch chuyển)
* Hàm `std::move` và `std::forward`
* Cơ chế copy‑and‑swap

### Đặc tính kiểu dữ liệu và Thư viện `type_traits`

* Các kiểm tra `is_integral`, `is_class`, `enable_if`, v.v.
* Nguyên lý SFINAE (Substitution Failure Is Not An Error - Thất bại khi thế kiểu dữ liệu không phải là lỗi) – khái niệm cơ bản
* Toán tử `decltype` và cơ chế suy diễn kiểu `auto`

### Khai báo Hằng số tự định nghĩa (User‑Defined Literals)

* Tạo hằng số tùy biến (ví dụ: `"hello"_s` để biểu thị kiểu dữ liệu `std::string`)

### Lập trình Đồng thời cơ bản (Concurrency Basics) (C++11 trở đi)

* Lớp `std::thread`, `std::jthread` (C++20)
* Cơ chế khóa đồng bộ: `std::mutex`, `std::lock_guard`, `std::unique_lock`
* Thao tác bất đồng bộ: `std::async` và `std::future` / `std::promise`
* Các kiểu dữ liệu nguyên tử (`std::atomic`)

---

## Chương 11: Các mẫu Thiết kế trong C++ (Design Patterns - Tổng quan)

* Mẫu khởi tạo (Creational patterns):
  * Singleton (phát triển các phiên bản an toàn đa luồng thread‑safe)
  * Phương thức nhà máy (Factory Method), Nhà máy trừu tượng (Abstract Factory)
  * Xây dựng (Builder)
* Mẫu cấu trúc (Structural patterns):
  * Chuyển đổi (Adapter - sử dụng tính kế thừa kết hợp với tính đóng gói cấu thành)
  * Trang trí (Decorator)
  * Đại diện (Proxy)
* Mẫu hành vi (Behavioural patterns):
  * Quan sát (Observer - sử dụng cơ chế gọi lại callback qua `std::function`)
  * Chiến lược (Strategy)
  * Lệnh (Command)
  * Trạng thái (State)
* Mẫu quản lý tài nguyên tự động RAII

---

## Chương 12: Quy chuẩn lập trình và Các thiết kế mẫu (Best Practices and Idioms)

* **Thiết kế RAII** – mọi tài nguyên hệ thống bắt buộc phải được đóng gói gọn trong một đối tượng
* **Copy‑and‑Swap** nhằm bảo đảm an toàn ngoại lệ tuyệt đối
* **Mẫu thiết kế Pimpl** (Pointer to Implementation - Con trỏ đến phần thực thi) – giúp giảm thiểu sự phụ thuộc mã nguồn lúc biên dịch
* **Mẫu thiết kế NVI** (Non‑virtual Interface - Giao diện phi ảo)
* **Mẫu thiết kế CRTP** (Curiously Recurring Template Pattern - Mẫu khuôn mẫu lặp lại kỳ lạ) – ứng dụng đa hình tĩnh
* **Quy tắc số Không (Rule of Zero)** – để trình biên dịch tự động sinh ra các phương thức đặc biệt
* Sử dụng từ khóa `const` ở bất cứ nơi nào có thể (tính đúng đắn hằng số const correctness)
* Ưu tiên sử dụng `nullptr` thay vì các hằng số cũ `NULL` hoặc `0`
* Ưu tiên sử dụng `enum class` (strongly typed enum) thay vì kiểu `enum` truyền thống
* Tránh sử dụng các lệnh cấp phát thô `new`/`delete` – hãy làm quen với con trỏ thông minh và các bộ lưu trữ container chuẩn
* Tận dụng tối đa các từ khóa đặc tả `override` và `final`

---

## Chương 13: Gỡ lỗi, Kiểm thử và Công cụ (Debugging, Testing, and Tools)

* Quy trình biên dịch mã nguồn: tiền xử lý (preprocessing), biên dịch (compilation), hợp dịch (assembly), liên kết (linking)
* Các tùy chọn biên dịch thông dụng: `-Wall`, `-Wextra`, `-Werror`, `-std=c++17`
* Gỡ lỗi mã nguồn bằng công cụ `gdb` hoặc `lldb`
* Các công cụ phát hiện lỗi bảo mật bộ nhớ (Sanitizers): AddressSanitizer, UndefinedBehaviorSanitizer
* Thư viện kiểm thử đơn vị (Unit testing frameworks): Google Test, Catch2
* Định dạng mã nguồn chuẩn: `clang-format`
* Phân tích mã nguồn tĩnh: `clang-tidy`, `cppcheck`
* Đo đạc hiệu năng ứng dụng (Profiling): `gprof`, `perf`, `valgrind`

---

## Chương 14: Các Dự án Thực hành và Bài tập mẫu (Hands‑on Projects)

### Mức độ Cơ bản (Beginner)

* Hệ thống quản lý tài khoản ngân hàng (luyện tập tính đóng gói, hàm khởi tạo)
* Xây dựng lớp biểu diễn Đa thức (Polynomial) tích hợp quá tải toán tử
* Thiết kế khuôn mẫu mảng động tổng quát (tương tự như `std::vector`)

### Mức độ Trung cấp (Intermediate)

* Tự cài đặt lớp biểu diễn xâu ký tự `String` (sao chép sâu, cơ chế dịch chuyển, tùy chọn đếm tham chiếu)
* Xây dựng hệ thống phân cấp các Hình học (lớp cơ sở trừu tượng, vẽ đa hình)
* Trình đánh giá biểu thức toán học (sử dụng cấu trúc ngăn xếp, hàm đối tượng hoặc tính kế thừa)
* Tự cài đặt con trỏ thông minh đơn giản `shared_ptr` (khuôn mẫu lớp, cơ chế đếm tham chiếu)

### Mức độ Nâng cao (Advanced)

* Hàng đợi an toàn đa luồng (Thread‑safe queue) giải quyết bài toán Sản xuất - Tiêu thụ (Producer‑Consumer)
* Thiết kế bộ lưu trữ Container thu nhỏ theo chuẩn STL (tích hợp bộ duyệt iterator, hỗ trợ bộ cấp phát allocator)
* Hệ thống quản lý sự kiện sử dụng mẫu thiết kế Quan sát (Observer) kết hợp `std::function`
* Bộ quản lý bộ nhớ đệm (Memory pool) / Tự thiết kế bộ cấp phát bộ nhớ (custom allocator)

---

## Chương 15: So sánh C++ với các ngôn ngữ hướng đối tượng khác

* **C++ so với Java**:
  * Đa kế thừa, quá tải toán tử, cơ chế con trỏ trực tiếp
  * Hàm hủy (Destructors) so với bộ thu gom rác tự động (Garbage Collection)
  * Đối tượng cấp phát trực tiếp trên ngăn xếp Stack (không bắt buộc phải dùng `new` cho mọi đối tượng)
* **C++ so với C#**:
  * Thuộc tính Properties so với các cặp phương thức Getter/Setter thông thường
  * Kiểu tham trị (value types) và kiểu tham chiếu (reference types)
* **C++ so với Python**:
  * Định kiểu tĩnh (Static typing) so với định kiểu động (Dynamic typing)
  * Hiệu năng thực thi vượt trội của ngôn ngữ biên dịch trực tiếp
* **Những lĩnh vực C++ chiếm ưu thế tuyệt đối**: lập trình hệ thống (system programming), ứng dụng thời gian thực (real‑time applications), công cụ phát triển game (game engines), hệ thống nhúng (embedded systems).

---

## Ủng hộ dự án (Support)

Nếu khung chương trình này giúp bạn làm chủ Lập trình hướng đối tượng trong C++, hãy ⭐ tặng một sao cho kho lưu trữ này. Mọi sự đóng góp, gợi ý và sửa lỗi từ phía các bạn luôn được chào đón nhiệt tình. Chúc các bạn lập trình vui vẻ!
