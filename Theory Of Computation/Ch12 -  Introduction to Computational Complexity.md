## Giới thiệu về Độ phức tạp tính toán (Introduction to Computational Complexity)

Độ phức tạp tính toán nghiên cứu **các tài nguyên cần thiết để giải các bài toán tính toán**, chủ yếu là **thời gian** và **không gian**, như là hàm của kích thước đầu vào. Nó là một phần cốt lõi của Lý thuyết Tính toán và giúp phân loại các bài toán dựa trên độ khó vốn có của chúng.

---

## Độ phức tạp thời gian

### Định nghĩa

Độ phức tạp thời gian đo **số phép toán cơ bản** mà một thuật toán thực hiện như là hàm của kích thước đầu vào $n$.

### Các trường hợp phổ biến

* **Trường hợp tốt nhất**: Thời gian tối thiểu
* **Trường hợp trung bình**: Thời gian kỳ vọng
* **Trường hợp xấu nhất**: Thời gian tối đa (quan trọng nhất trong phân tích)

### Ví dụ

* Tìm kiếm tuyến tính → $O(n)$
* Tìm kiếm nhị phân → $O(\log n)$
* Sắp xếp nổi bọt → $O(n^2)$

### Nhận thức chính

Chúng ta tập trung vào **tốc độ tăng trưởng**, không phải thời gian thực thi chính xác.

---

## Độ phức tạp không gian

### Định nghĩa

Độ phức tạp không gian đo **tổng bộ nhớ được sử dụng** bởi một thuật toán, bao gồm:

* Không gian đầu vào
* Không gian phụ trợ (bộ nhớ thêm)

### Ví dụ

* Thuật toán lặp → $O(1)$ không gian
* Fibonacci đệ quy → $O(n)$ không gian ngăn xếp

### Nhận thức chính

Các thuật toán hiệu quả cân bằng **sự đánh đổi thời gian vs bộ nhớ**.

---

## Ký hiệu tiệm cận

### Mục đích

Để mô tả **sự tăng trưởng của các hàm** với kích thước đầu vào lớn.

### Các ký hiệu quan trọng

#### 1. Ký hiệu Big-O $O(f(n))$

* Giới hạn trên (trường hợp xấu nhất)
* Ví dụ: $O(n^2)$

#### 2. Ký hiệu Omega $\Omega(f(n))$

* Giới hạn dưới (trường hợp tốt nhất)

#### 3. Ký hiệu Theta $\Theta(f(n))$

* Giới hạn chặt (tăng trưởng chính xác)

### Ví dụ

Nếu thuật toán mất $3n^2 + 5n + 2$, thì:

* $O(n^2)$
* $\Omega(n^2)$
* $\Theta(n^2)$

---

## Các lớp độ phức tạp

### Định nghĩa

Các lớp độ phức tạp nhóm các bài toán dựa trên **tài nguyên cần thiết để giải chúng**.

---

## Lớp P (Thời gian đa thức)

### Định nghĩa

Lớp **P** chứa các bài toán có thể được giải trong **thời gian đa thức** bởi một máy đơn định.

### Ví dụ

* Sắp xếp (Merge Sort → $O(n \log n)$)
* Đường đi ngắn nhất (Thuật toán Dijkstra)

### Ý tưởng chính

* Các bài toán hiệu quả và giải được

---

## Lớp NP (Thời gian đa thức không đơn định)

### Định nghĩa

Lớp **NP** chứa các bài toán mà nghiệm của chúng có thể được **xác minh trong thời gian đa thức**.

### Ý tưởng chính

* Tìm nghiệm có thể khó
* Xác minh nghiệm dễ dàng

### Ví dụ

* Cho tập con, xác minh xem tổng có bằng mục tiêu không (bài toán Tổng tập con – Subset Sum)

---

## Quan hệ giữa P và NP

* $P \subseteq NP$
* Bài toán mở lớn:
  **$P = NP$?** (chưa được giải quyết)

---

## Bài toán NP-Hard

### Định nghĩa

Bài toán là **NP-Hard** nếu:

* Nó ít nhất khó như những bài toán khó nhất trong NP
* Nó có thể hoặc không thuộc NP

### Điểm chính

* Không yêu cầu xác minh đa thức
* Có thể còn khó hơn bài toán NP

### Ví dụ

* Bài toán dừng (Halting Problem)

---

## Bài toán NP-Complete (NP-Đầy đủ)

### Định nghĩa

Bài toán là **NP-Complete** nếu:

1. Nó thuộc NP
2. Nó là NP-Hard

### Nhận thức chính

* Các bài toán khó nhất trong NP
* Nếu một bài toán NP-Complete được giải trong thời gian đa thức →
  Tất cả bài toán NP đều trở thành đa thức

### Ví dụ

* Bài toán người bán hàng (phiên bản quyết định)
* Bài toán thỏa mãn Boolean (SAT)

---

## Quan hệ giữa các lớp

### Cấu trúc chứa nhau

- **P ⊆ NP**  
  Các bài toán giải được trong thời gian đa thức cũng có thể xác minh trong thời gian đa thức.

- **NP-Complete ⊆ NP**  
  Các bài toán NP-Complete là tập con của NP.

- **NP-Hard ⊇ NP-Complete**  
  Các bài toán NP-Hard bao gồm tất cả bài toán NP-Complete (và có thể nhiều hơn).

### Diễn giải trực quan

* NP-Complete nằm ở giao điểm của NP và NP-Hard
* NP-Hard bao gồm các bài toán ngoài NP

---

## Bảng tóm tắt

| Lớp | Định nghĩa | Tính chất chính |
| ----------- | ------------------------------- | ---------------- |
| P | Giải được trong thời gian đa thức | Hiệu quả |
| NP | Xác minh được trong thời gian đa thức | Dễ kiểm tra |
| NP-Hard | Ít nhất khó bằng bài toán NP | Có thể không thuộc NP |
| NP-Complete | NP + NP-Hard | Khó nhất trong NP |

---

## Hiểu biết cuối cùng

* **Độ phức tạp thời gian & không gian** → Đo lường hiệu quả
* **Ký hiệu tiệm cận** → Mô tả tăng trưởng
* **P vs NP** → Hiệu quả vs xác minh
* **NP-Complete** → Các bài toán quan trọng nhất trong KHMT

---
