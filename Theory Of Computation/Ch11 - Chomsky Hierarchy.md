## Phân cấp Chomsky (Chomsky Hierarchy)

**Phân cấp Chomsky**, được đề xuất bởi Noam Chomsky, là một phân loại cơ bản các ngôn ngữ hình thức dựa trên **hạn chế của văn phạm** và **năng lực tính toán cần thiết để nhận biết chúng**. Nó là trung tâm của Lý thuyết Tính toán và thiết kế trình biên dịch.

Phân cấp bao gồm bốn cấp độ:

* Kiểu 3: Ngôn ngữ chính quy
* Kiểu 2: Ngôn ngữ phi ngữ cảnh
* Kiểu 1: Ngôn ngữ nhạy ngữ cảnh
* Kiểu 0: Ngôn ngữ đệ quy liệt kê

---

## Kiểu 3: Ngôn ngữ chính quy

### Định nghĩa

Ngôn ngữ chính quy là lớp ngôn ngữ đơn giản nhất, được định nghĩa bởi **văn phạm chính quy** và được nhận biết bởi **ô-tô-mát hữu hạn**.

### Quy tắc văn phạm (Văn phạm chính quy)

Hai dạng:

* Tuyến tính phải:
  A → aB | a
* Tuyến tính trái:
  A → Ba | a

### Mô hình tính toán

* Được nhận biết bởi:

  * Ô-tô-mát hữu hạn đơn định (DFA)
  * Ô-tô-mát hữu hạn không đơn định (NFA)

### Ví dụ

Ngôn ngữ:
L = { các chuỗi trên {0,1} kết thúc bằng "01" }

Biểu thức chính quy:

```
(0|1)*01
```

### Đặc điểm chính

* Không có bộ nhớ ngoài trạng thái hiện tại
* Không thể xử lý các mẫu lồng nhau hoặc đệ quy
* Rất hiệu quả

### Ứng dụng

* Phân tích từ vựng trong trình biên dịch
* So khớp mẫu (ví dụ: công cụ regex)

---

## Kiểu 2: Ngôn ngữ phi ngữ cảnh (CFL)

### Định nghĩa

Các ngôn ngữ được sinh bởi **văn phạm phi ngữ cảnh (CFG)** trong đó mỗi quy tắc sinh có một ký hiệu không kết cuối đơn ở vế trái.

### Quy tắc văn phạm

```
A → γ
```

Trong đó:

* A = ký hiệu không kết cuối
* γ = chuỗi các kết cuối và/hoặc không kết cuối

### Mô hình tính toán

* Được nhận biết bởi **Ô-tô-mát đẩy xuống (PDA)**
* Sử dụng **ngăn xếp** cho bộ nhớ

### Ví dụ

Ngôn ngữ:
L = { aⁿbⁿ | n ≥ 0 }

Văn phạm:

```
S → aSb | ε
```

### Đặc điểm chính

* Có thể biểu diễn các cấu trúc lồng nhau
* Bộ nhớ hạn chế qua ngăn xếp
* Không thể xử lý nhiều phụ thuộc (ví dụ: aⁿbⁿcⁿ)

### Ứng dụng

* Phân tích cú pháp (parsing)
* Thiết kế ngôn ngữ lập trình

---

## Kiểu 1: Ngôn ngữ nhạy ngữ cảnh (CSL)

### Định nghĩa

Các ngôn ngữ được sinh bởi **văn phạm nhạy ngữ cảnh**, trong đó các quy tắc sinh phụ thuộc vào các ký hiệu xung quanh (ngữ cảnh).

### Quy tắc văn phạm

```
αAβ → αγβ
```

Ràng buộc:

* Độ dài đầu ra ≥ độ dài đầu vào (không thu hẹp)

### Mô hình tính toán

* Được nhận biết bởi **Ô-tô-mát giới hạn tuyến tính (LBA)**
* Kích thước băng bị giới hạn theo độ dài đầu vào

### Ví dụ

Ngôn ngữ:
L = { aⁿbⁿcⁿ | n ≥ 1 }

Ngôn ngữ này yêu cầu:

* Số lượng a, b và c bằng nhau
* Không thể với CFG

### Đặc điểm chính

* Mạnh hơn CFL
* Có thể thực thi nhiều phụ thuộc
* Yêu cầu bộ nhớ có giới hạn nhưng không tầm thường

---

## Kiểu 0: Ngôn ngữ đệ quy liệt kê

### Định nghĩa

Lớp ngôn ngữ tổng quát nhất, được sinh bởi **văn phạm không hạn chế** và được nhận biết bởi Máy Turing.

### Quy tắc văn phạm

```
α → β
```

(Không hạn chế, ngoại trừ α không rỗng)

### Mô hình tính toán

* Máy Turing (băng vô hạn, đầu đọc/ghi)

### Ví dụ

Ngôn ngữ:
L = { w#w | w ∈ {0,1}* }

### Đặc điểm chính

* Năng lực tính toán tối đa
* Có thể không dừng với một số đầu vào
* Bao gồm các bài toán không quyết định được

### Ứng dụng

* Tính toán tổng quát
* Hình thức hóa thuật toán

---

## Quan hệ giữa các Lớp ngôn ngữ

### Tính chất chứa nhau

Phân cấp lồng nhau đúng nghĩa:

```
Chính quy ⊂ Phi ngữ cảnh ⊂ Nhạy ngữ cảnh ⊂ Đệ quy liệt kê
```

### Diễn giải

* Mọi **ngôn ngữ chính quy** đều là **phi ngữ cảnh**
* Mọi **ngôn ngữ phi ngữ cảnh** đều là **nhạy ngữ cảnh**
* Mọi **ngôn ngữ nhạy ngữ cảnh** đều là **đệ quy liệt kê**
* Chiều ngược lại không đúng

---

## Bảng tóm tắt so sánh

| Kiểu | Lớp ngôn ngữ | Hạn chế văn phạm | Mô hình máy | Loại bộ nhớ |
| ---- | ---------------------- | ------------------------- | ------------------------ | ------------ |
| 0 | Đệ quy liệt kê | Không hạn chế | Máy Turing | Vô hạn |
| 1 | Nhạy ngữ cảnh | Độ dài không giảm | Ô-tô-mát giới hạn tuyến tính | Băng có giới hạn |
| 2 | Phi ngữ cảnh | Một không kết cuối (VT) | Ô-tô-mát đẩy xuống | Ngăn xếp |
| 3 | Chính quy | Quy tắc tuyến tính | Ô-tô-mát hữu hạn | Không có bộ nhớ |

---

## Hiểu biết khái niệm

* **Ngôn ngữ chính quy** → Các mẫu đơn giản, không có bộ nhớ
* **Ngôn ngữ phi ngữ cảnh** → Các cấu trúc lồng nhau (ví dụ: dấu ngoặc)
* **Ngôn ngữ nhạy ngữ cảnh** → Nhiều phụ thuộc
* **Ngôn ngữ đệ quy liệt kê** → Tính toán tổng quát

---
