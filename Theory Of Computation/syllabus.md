# 📘 LÝ THUYẾT TÍNH TOÁN – Đề cương môn học

## Chương 1. Giới thiệu về Lý thuyết Tính toán

- Lý thuyết Tính toán là gì? (Ô-tô-mát, Khả năng tính toán, Độ phức tạp)
- Động lực và ứng dụng: trình biên dịch, kiểm chứng phần mềm, trí tuệ nhân tạo, mật mã học
- Bảng chữ cái (alphabet), chuỗi (string) và ngôn ngữ (language) – các định nghĩa cơ bản
- Các phép toán trên chuỗi: ghép nối, đảo ngược, chuỗi con, độ dài
- Các phép toán trên ngôn ngữ: hợp, giao, ghép nối, bù, bao đóng Kleene, bao đóng dương
- Đồng cấu (homomorphism) và đồng cấu ngược (inverse homomorphism)

---

## Chương 2. Ô-tô-mát hữu hạn (Finite Automata)

- Ô-tô-mát hữu hạn đơn định (DFA – Deterministic Finite Automata)
  - Định nghĩa hình thức (bộ 5 thành phần)
  - Hàm chuyển trạng thái và sơ đồ trạng thái
  - Ngôn ngữ được chấp nhận bởi DFA
- Ô-tô-mát hữu hạn không đơn định (NFA – Non‑Deterministic Finite Automata)
  - Định nghĩa và lý do tại sao tính không đơn định hữu ích
  - ε‑NFA và ε‑bao đóng (ε‑closure)
- Tính tương đương của DFA, NFA và ε‑NFA (phác thảo chứng minh)
- Chuyển đổi NFA sang DFA (phương pháp xây dựng tập con)
- Thu gọn DFA – Thuật toán điền bảng (Thuật toán Hopcroft) & Định lý Myhill–Nerode
- Thiết kế ô-tô-mát hữu hạn cho các ngôn ngữ cho trước

---

## Chương 3. Ngôn ngữ chính quy và Biểu thức chính quy

- Ngôn ngữ chính quy và các tính chất của chúng
- Biểu thức chính quy: cú pháp và ngữ nghĩa (ε, ∅, a, R1|R2, R1·R2, R*)
- Chuyển đổi biểu thức chính quy sang ô-tô-mát hữu hạn (xây dựng Thompson)
- Chuyển đổi ô-tô-mát hữu hạn sang biểu thức chính quy (loại bỏ trạng thái, định lý Arden)
- Tính đóng của ngôn ngữ chính quy (hợp, giao, bù, ghép nối, bao đóng Kleene, đảo ngược, đồng cấu)
- Tính chất quyết định: tính rỗng, tính hữu hạn, thuộc tư cách thành viên, tính tương đương

---

## Chương 4. Bổ đề bơm cho Ngôn ngữ chính quy

- Phát biểu của bổ đề bơm
- Ý tưởng chứng minh (nguyên lý hộp thư – pigeonhole principle – trên các trạng thái DFA)
- Áp dụng bổ đề bơm – chứng minh một ngôn ngữ **không phải** ngôn ngữ chính quy
- Các kỹ thuật chứng minh tính không chính quy (bơm, tính đóng, Myhill–Nerode)
- Hạn chế của bổ đề bơm

---

## Chương 5. Văn phạm phi ngữ cảnh (CFG – Context‑Free Grammars)

- Văn phạm phi ngữ cảnh – các thành phần (V, Σ, R, S)
- Dẫn xuất và dạng câu
- Dẫn xuất trái nhất và dẫn xuất phải nhất
- Cây phân tích cú pháp (parse tree)
- Sự mơ hồ trong văn phạm phi ngữ cảnh (loại bỏ sự mơ hồ khi có thể)
- Ngôn ngữ mơ hồ vốn dĩ (inherently ambiguous languages)

---

## Chương 6. Đơn giản hóa và Dạng chuẩn của CFG

- Loại bỏ ε-production (ký hiệu nullable)
- Loại bỏ unit production
- Loại bỏ ký hiệu vô dụng (không sinh, không đạt được)
- Dạng chuẩn Chomsky (CNF – Chomsky Normal Form) – định nghĩa và thuật toán chuyển đổi
- Dạng chuẩn Greibach (GNF – Greibach Normal Form) – định nghĩa và chuyển đổi (khái niệm)
- Chuyển đổi bất kỳ CFG nào sang CNF (từng bước)

---

## Chương 7. Ô-tô-mát đẩy xuống (PDA – Pushdown Automata)

- Định nghĩa và các thành phần của PDA (bộ 7)
- Các phép toán ngăn xếp và mô tả tức thời (ID)
- Chấp nhận bởi **ngăn xếp rỗng** so với **trạng thái kết thúc** – tính tương đương
- Tính tương đương của PDA và CFG:
  - CFG → PDA (xây dựng một trạng thái)
  - PDA → CFG (từ chấp nhận ngăn xếp rỗng)
- Xây dựng PDA cho các ngôn ngữ cho trước

---

## Chương 8. Bổ đề bơm cho Ngôn ngữ phi ngữ cảnh

- Phát biểu của bổ đề bơm cho CFL (dựa trên chiều cao của cây phân tích)
- Áp dụng – chứng minh các ngôn ngữ **không phải** ngôn ngữ phi ngữ cảnh
- Hạn chế của bổ đề bơm CFL (đề cập đến bổ đề Ogden)

---

## Chương 9. Máy Turing (Turing Machines)

- Định nghĩa và các thành phần của Máy Turing (bộ 7: Q, Σ, Γ, δ, q₀, q_accept, q_reject)
- Mô tả tức thời (ID) và cách TM tính toán
- Ngôn ngữ được chấp nhận bởi Máy Turing (dừng ở trạng thái chấp nhận)
- Thiết kế Máy Turing cho các ngôn ngữ đơn giản
- Các biến thể của Máy Turing:
  - Máy Turing nhiều băng (tương đương với một băng)
  - Máy Turing không đơn định (tương đương)
  - Nhiều đầu đọc, băng hai chiều (sơ lược)
- Luận đề Church–Turing

---

## Chương 10. Quyết định được và Không quyết định được

- Ngôn ngữ đệ quy (quyết định được) so với đệ quy liệt kê (r.e.)
- Ngôn ngữ quyết định được – TM luôn dừng có thể quyết định thuộc tư cách thành viên
- Ngôn ngữ không quyết định được – không có TM nào quyết định được
- Bài toán dừng (halting problem) – phát biểu và chứng minh không quyết định được (chéo hóa)
- Quy về (reductions) và kỹ thuật quy về (quy về nhiều-một / quy về ánh xạ)
- Định lý Rice – mọi tính chất không tầm thường của ngôn ngữ r.e. đều không quyết định được
- Chứng minh tính không quyết định được cho tính rỗng, tính chính quy và Bài toán tương ứng Post (PCP)

---

## Chương 11. Phân cấp Chomsky (Chomsky Hierarchy)

- Kiểu 0: Ngôn ngữ đệ quy liệt kê (Máy Turing)
- Kiểu 1: Ngôn ngữ nhạy ngữ cảnh (LBA – Linear Bounded Automata)
  - Định nghĩa của LBA (Máy Turing với băng bị giới hạn)
  - Ví dụ: `{aⁿbⁿcⁿ | n≥0}` là ngôn ngữ nhạy ngữ cảnh
- Kiểu 2: Ngôn ngữ phi ngữ cảnh (Ô-tô-mát đẩy xuống)
- Kiểu 3: Ngôn ngữ chính quy (Ô-tô-mát hữu hạn)
- Quan hệ giữa các lớp ngôn ngữ (chứa nhau đúng: Chính quy ⊂ CFL ⊂ CSL ⊂ Đệ quy ⊂ RE)
- Tính đóng và tính chất quyết định qua phân cấp

---

## Chương 12. Giới thiệu về Độ phức tạp tính toán

- Độ phức tạp thời gian – số bước trong trường hợp xấu nhất
- Độ phức tạp không gian – số ô băng trong trường hợp xấu nhất
- Ký hiệu tiệm cận (Big‑O, Big‑Ω, Big‑Θ) áp dụng cho tài nguyên TM
- Các lớp độ phức tạp:
  - P (thời gian đa thức)
  - NP (thời gian đa thức không đơn định)
  - NP‑Hard và NP‑Complete (mức độ giới thiệu)
  - Đề cập sơ qua PSPACE, L, NL
- Quy về thời gian đa thức (≤_p)
- Định lý Cook–Levin (SAT là NP-đầy đủ) – phát biểu
- Ví dụ về các bài toán NP-đầy đủ: 3‑SAT, Clique, Vertex Cover, Subset Sum

---

## Tóm tắt

Đề cương này bao gồm toàn bộ phạm vi chính thức của Lý thuyết Tính toán cần thiết cho:

- Các kỳ thi đại học
- Phỏng vấn khoa học máy tính cốt lõi
- Nền tảng nghiên cứu về khả năng tính toán và độ phức tạp

Bao gồm tất cả các chủ đề thiết yếu: ô-tô-mát hữu hạn, ngôn ngữ chính quy, bổ đề bơm, văn phạm phi ngữ cảnh, ô-tô-mát đẩy xuống, Máy Turing, tính quyết định, định lý Rice, phân cấp Chomsky và giới thiệu về độ phức tạp tính toán (P, NP, NP-đầy đủ).

---

## ⭐ Ủng hộ

Nếu đề cương này giúp bạn cấu trúc việc học hoặc kho lưu trữ GitHub của bạn, hãy **star** để ủng hộ.  
Đóng góp và sửa lỗi luôn được chào đón.
