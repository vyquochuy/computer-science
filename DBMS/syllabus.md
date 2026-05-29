# Giáo trình Hệ quản trị Cơ sở dữ liệu (DBMS Syllabus)

## Chương 1: Cơ sở lý thuyết về Cơ sở dữ liệu (Database Fundamentals)

* Tổng quan về Hệ quản trị cơ sở dữ liệu (DBMS - Database Management System)
* Ưu điểm của DBMS so với hệ thống quản lý tệp tin truyền thống (file system)
* Các mức trừu tượng hóa dữ liệu (Data abstraction):
  * Mức vật lý (*Physical level*)
  * Mức logic (*Logical level*)
  * Mức khung nhìn (*View level*)
* Tính độc lập dữ liệu (Data independence)
* Lược đồ và Thực thể (Schema vs Instance)

---

## Chương 2: Mô hình quan hệ (Relational Model)

* Quan hệ (*Relation*), Bộ (*Tuple*), Thuộc tính (*Attribute*)
* Các loại khóa (Keys):
  * Siêu khóa (*Super key*)
  * Khóa ứng viên (*Candidate key*)
  * Khóa chính (*Primary key*)
  * Khóa ngoại (*Foreign key*)
* Ràng buộc toàn vẹn (Integrity constraints)
* Lược đồ quan hệ (Relational schema)

---

## Chương 3: Đại số quan hệ (Relational Algebra)

* Phép chọn (*Selection* - σ)
* Phép chiếu (*Projection* - π)
* Phép hợp (*Union*), Phép giao (*Intersection*), Phép hiệu (*Difference*)
* Tích Descartes (*Cartesian product*)
* Phép kết (Joins):
  * Phép kết tự nhiên (*Natural join*)
  * Phép kết Theta (*Theta join*)
  * Phép kết ngoài (*Outer join*)
* Phép chia (*Division operator*)

---

## Chương 4: Ngôn ngữ truy vấn cấu trúc SQL (SQL)

* Nhóm ngôn ngữ định nghĩa dữ liệu (DDL): CREATE, ALTER, DROP
* Nhóm ngôn ngữ thao tác dữ liệu (DML): SELECT, INSERT, UPDATE, DELETE
* Các phép kết trong SQL (inner, outer joins)
* Các câu truy vấn lồng nhau (Nested queries)
* Hàm gộp và gom nhóm (Aggregation):
  * Mệnh đề GROUP BY
  * Mệnh đề HAVING
* Khung nhìn (Views)
* Các ràng buộc dữ liệu (Constraints)

---

## Chương 5: Mô hình thực thể - mối quan hệ (ER Model)

* Thực thể (*Entity*), thuộc tính (*attribute*)
* Mối quan hệ (*Relationship*)
* Bản số / Lực lượng (*Cardinality*) (1:1, 1:N, M:N)
* Ràng buộc tham gia (*Participation constraints*)
* Thực thể yếu (*Weak entity*)
* Quy trình chuyển đổi từ mô hình ER sang mô hình quan hệ (ER to relational mapping)

---

## Chương 6: Phụ thuộc hàm (Functional Dependencies)

* Phụ thuộc hàm (FD - Functional dependency)
* Phụ thuộc hàm tầm thường và không tầm thường (Trivial and non-trivial FD)
* Bao đóng của tập thuộc tính (Closure of attributes)
* Thuật toán tìm khóa (Finding keys)
* Hệ tiên đề Armstrong (Armstrong’s axioms)

---

## Chương 7: Chuẩn hóa cơ sở dữ liệu (Normalization)

* Các dạng chuẩn: 1NF, 2NF, 3NF, BCNF
* Dạng chuẩn 4 (4NF - khái niệm cơ bản)
* Phân rã không mất mát thông tin (Lossless decomposition)
* Bảo toàn phụ thuộc (Dependency preservation)

---

## Chương 8: Giao dịch (Transactions)

* Khái niệm giao dịch (*Transaction*)
* Các thuộc tính ACID (ACID properties)
* Các trạng thái của giao dịch (Transaction states)

---

## Chương 9: Điều khiển đồng thời (Concurrency Control)

* Lịch trình thực thi (Schedules) (tuần tự (*serial*), không tuần tự (*non-serial*))
* Khả năng tuần tự hóa (Serializability):
  * Khả năng tuần tự hóa xung đột (*Conflict serializability*)
  * Khả năng tuần tự hóa khung nhìn (*View serializability*)
* Giao thức điều khiển dựa trên khóa (Lock-based protocols)
* Khóa hai pha (2PL - Two-phase locking)
* Bế tắc (Deadlocks) (phương pháp phát hiện, ngăn ngừa)

---

## Chương 10: Hệ thống phục hồi (Recovery System)

* Phân loại lỗi hệ thống (Types of failures)
* Phục hồi dựa trên nhật ký (Log-based recovery)
* Hoàn tác và Làm lại (Undo and Redo)
* Điểm kiểm tra (Checkpoints)

---

## Chương 11: Đánh chỉ mục (Indexing)

* Khái niệm cơ bản về đánh chỉ mục (Indexing basics)
* Chỉ mục chính và chỉ mục phụ (Primary and secondary index)
* Chỉ mục dày đặc và chỉ mục thưa thớt (Dense and sparse index)
* Cấu trúc cây B (B-tree) và B+ tree

---

## Chương 12: Tổ chức tệp tin và Băm (File Organization & Hashing)

* Các phương pháp tổ chức tệp tin (File organization):
  * Tổ chức kiểu đống (*Heap*)
  * Tổ chức tuần tự (*Sequential*)
* Kỹ thuật băm (Hashing):
  * Băm tĩnh (*Static hashing*)
  * Băm động (*Dynamic hashing*)

---

## Chương 13: Lưu trữ và Quản lý vùng đệm (Storage & Buffer Management)

* Cấu trúc lưu trữ của đĩa (Disk storage structure)
* Quản lý vùng đệm (Buffer management)
* Các kỹ thuật truy cập dữ liệu (Access techniques)

---

# Lộ trình học tập khuyến nghị (Smart Study Order)

1 → 2 → 3 → 4 → 6 → 7 → 8 → 9 → 10 → 11 → 5 → 12 → 13

---

## ⭐ Ủng hộ (Support)

Nếu kho lưu trữ này mang lại giá trị cho việc học tập của bạn, hãy cân nhắc tặng 1 ⭐ để ủng hộ dự án.
