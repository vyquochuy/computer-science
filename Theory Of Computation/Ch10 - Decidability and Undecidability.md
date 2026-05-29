# Quyết định được và Không quyết định được (Decidability and Undecidability)

Tính quyết định là một khái niệm trung tâm trong lý thuyết tính toán phân loại các bài toán dựa trên việc chúng có thể được giải bằng thuật toán hay không. Nó xây dựng trực tiếp trên năng lực tính toán của Máy Turing.

---

## 1. Ngôn ngữ đệ quy và Đệ quy liệt kê

### Ngôn ngữ đệ quy (Quyết định được)

Ngôn ngữ ( L ) là **đệ quy** nếu tồn tại Máy Turing:

* Dừng với **mọi đầu vào**, và
* Chấp nhận nếu chuỗi thuộc ( L ), nếu không thì từ chối

**Tính chất chính:**
Mọi đầu vào đều dẫn đến câu trả lời rõ ràng (CÓ/KHÔNG).

---

### Ngôn ngữ đệ quy liệt kê (RE)

Ngôn ngữ ( L ) là **đệ quy liệt kê** nếu tồn tại Máy Turing:

* Chấp nhận các chuỗi trong ( L )
* Có thể **lặp mãi** với các chuỗi không thuộc ( L )

**Sự khác biệt chính:**

| Tính chất | Đệ quy | Đệ quy liệt kê |
| -------- | ------------- | ---------------------- |
| Dừng | Luôn dừng | Có thể không dừng |
| Quyết định | Luôn CÓ/KHÔNG | Chỉ đảm bảo CÓ |

---

### Quan hệ

$$
\text{Đệ quy} \subseteq \text{Đệ quy liệt kê}
$$

---

## 2. Ngôn ngữ quyết định được

Ngôn ngữ **quyết định được** nếu tồn tại Máy Turing quyết định nó.

### Đặc điểm:

* Luôn dừng
* Đưa ra câu trả lời CÓ/KHÔNG đúng
* Tương ứng với **ngôn ngữ đệ quy**

### Ví dụ:
$$
* ( L = \{ a^n b^n \mid n \geq 0 \} )
$$
* Kiểm tra xem một chuỗi có số lượng `a` bằng `b` theo sau hay không là quyết định được

---

## 3. Ngôn ngữ không quyết định được

Ngôn ngữ **không quyết định được** nếu:

* Không tồn tại Máy Turing có thể quyết định nó với mọi đầu vào

### Đặc điểm:

* Không có thuật toán nào có thể luôn đưa ra câu trả lời
* Một số đầu vào có thể gây ra vòng lặp vô hạn

### Ví dụ:

* Bài toán dừng (Halting Problem)
* Bài toán tương ứng Post (PCP)

---

## 4. Bài toán dừng (Halting Problem)

**Bài toán dừng** là một trong những bài toán không quyết định được nổi tiếng nhất, được giới thiệu bởi Alan Turing.

### Phát biểu bài toán

Cho:

* Máy Turing ( M )
* Chuỗi đầu vào ( w )

Xác định xem:

$$
M \text{ có dừng với đầu vào } w \text{ không}
$$

---

### Kết quả chính

* **Không có thuật toán chung** nào giải quyết Bài toán dừng với mọi đầu vào

---

### Ý tưởng chứng minh (Mâu thuẫn)

1. Giả sử tồn tại máy ( H ) giải quyết bài toán dừng
2. Xây dựng một máy khác mâu thuẫn với hành vi của nó
3. Dẫn đến sự không nhất quán logic

Do đó, máy như vậy không thể tồn tại.

---

## 5. Quy về và Các kỹ thuật quy về

Quy về là phương pháp mạnh mẽ được sử dụng để chứng minh tính không quyết định.

### Khái niệm

Nếu bài toán ( A ) có thể quy về bài toán ( B ):

$$
A \leq B
$$

* Nếu ( A ) không quyết định được, thì ( B ) cũng phải không quyết định được

---

### Trực giác

* Biến đổi bài toán này thành bài toán khác
* Giải ( A ) bằng cách sử dụng ( B )
* Nếu ( B ) có thể giải được, thì ( A ) cũng có thể giải được

---

### Các loại quy về

* Quy về ánh xạ (Mapping reduction)
* Quy về Turing (Turing reduction)

---

## 6. Các chứng minh về tính không quyết định

Để chứng minh bài toán không quyết định được:

### Phương pháp từng bước

1. Bắt đầu với bài toán không quyết định được đã biết (ví dụ: Bài toán dừng)
2. Quy về bài toán mục tiêu
3. Chứng minh phép biến đổi là hợp lệ
4. Kết luận bài toán mục tiêu không quyết định được

---

### Ví dụ chiến lược

Để chứng minh ngôn ngữ ( L ) không quyết định được:

* Giả sử ( L ) quyết định được
* Sử dụng nó để giải Bài toán dừng
* Điều này tạo ra mâu thuẫn
* Do đó, ( L ) không quyết định được

---

## 7. Các nhận xét quan trọng

* Tất cả ngôn ngữ quyết định được đều là đệ quy liệt kê
* Không phải tất cả ngôn ngữ đệ quy liệt kê đều quyết định được
* Tính không quyết định cho thấy giới hạn của tính toán
* Nhiều bài toán thực tế (ví dụ: kiểm chứng chương trình) không quyết định được

---

## 8. Tóm tắt

* Ngôn ngữ đệ quy tương ứng với các bài toán quyết định được
* Ngôn ngữ đệ quy liệt kê có thể không dừng với mọi đầu vào
* Bài toán dừng là nền tảng của tính không quyết định
* Quy về là công cụ chính để chứng minh tính không quyết định
* Các bài toán không quyết định được xác định ranh giới của tính toán theo thuật toán
