# THIẾT KẾ TRÌNH BIÊN DỊCH (COMPILER DESIGN)

## Chương 1: Giới thiệu về Trình biên dịch (Introduction to Compilers)

* Trình biên dịch (*Compiler*) là gì? (Sự khác biệt so với trình thông dịch - *interpreter* và trình hợp dịch - *assembler*)
* Nhu cầu sử dụng trình biên dịch: dịch từ mã nguồn (*source code*) sang mã máy đích (*target machine code*)
* Các pha chính của một trình biên dịch: phân tích từ vựng (*lexical*) → phân tích cú pháp (*syntax*) → phân tích ngữ nghĩa (*semantic*) → mã trung gian (*IR*) → sinh mã (*code gen*) → tối ưu hóa (*optimizer*)
* Lượt dịch (*Passes*): trình biên dịch một lượt (*single-pass*) so với trình biên dịch nhiều lượt (*multi-pass*)
* Phân vùng trước (*Front-end*) so với phân vùng sau (*Back-end*)
* Các công cụ xây dựng trình biên dịch (Lex, Yacc, v.v.)

---

## Chương 2: Phân tích Từ vựng (Lexical Analysis)

* Vai trò của bộ phân tích từ vựng (*lexical analyzer / scanner*)
* Cơ chế đệm đầu vào (*Input buffering*): sơ đồ một bộ đệm (*one-buffer*), hai bộ đệm (*two-buffer schemes*), lính canh (*sentinels*)
* Token, pattern (mẫu), và lexeme
* Mô tả đặc tả của token sử dụng biểu thức chính quy (*regular expressions*)
* Ơ-tô-mát hữu hạn (*Finite automata*):
  * Ơ-tô-mát hữu hạn đơn định (*Deterministic Finite Automata - DFA*)
  * Ơ-tô-mát hữu hạn phi đơn định (*Non‑deterministic Finite Automata - NFA*)
  * Thuật toán chuyển đổi: NFA → DFA (xây dựng tập con - *subset construction*)
  * Tối thiểu hóa DFA (Thuật toán Hopcroft – mặt khái niệm)
* Từ biểu thức chính quy sang DFA (trực tiếp và gián tiếp qua NFA)
* Lỗi từ vựng và phục hồi lỗi (chế độ hoảng loạn - *panic mode*)

---

## Chương 3: Phân tích Cú pháp (Syntax Analysis / Parsing)

### Ngữ pháp Phi ngữ cảnh (Context‑Free Grammars - CFG)

* Ký hiệu kết thúc (*terminals*), ký hiệu không kết thúc (*non‑terminals*), ký hiệu bắt đầu (*start symbol*), luật sinh (*productions*)
* Dẫn xuất (*Derivations*): dẫn xuất trái nhất (*leftmost*), dẫn xuất phải nhất (*rightmost*)
* Cây phân tích cú pháp (*parse tree*) và cây cú pháp trừu tượng (*abstract syntax tree - AST*)
* Tính nhập nhằng trong ngữ pháp – nguyên nhân và cách loại bỏ
* Loại bỏ đệ quy trái (*left recursion*)
* Nhân tử hóa trái (*Left factoring*)

### Phân tích cú pháp từ trên xuống (Top‑Down Parsing)

* Phân tích cú pháp đệ quy đi xuống (*Recursive descent parsing*) (có quay lui - *with backtracking*)
* Phân tích cú pháp dự đoán (*Predictive parsing*) (không quay lui - *without backtracking*)
* Tập FIRST và FOLLOW – cách xây dựng và ý nghĩa
* Ngữ pháp LL(1): điều kiện, xây dựng bảng phân tích cú pháp
* Thuật toán phân tích cú pháp LL(1) (dựa trên ngăn xếp - *stack‑based*)
* Phục hồi lỗi trong LL(1)

### Phân tích cú pháp từ dưới lên (Bottom‑Up Parsing)

* Thu gọn tay cầm (*Handle pruning*), tay cầm (*handles*), tiền tố khả thi (*viable prefixes*)
* Tổng quan về phân tích cú pháp LR: các item, closure (bao đóng), goto
* Các item LR(0) và ơ-tô-mát LR(0)
* Phân tích cú pháp SLR(1) – LR đơn giản (được hỏi nhiều nhất trong đề thi GATE)
* Các item LR(1) chính tắc (Canonical LR(1))
* Phân tích cú pháp LALR(1) (lookahead LR)
* Xung đột dịch-thu gọn (*Shift‑reduce conflicts*), xung đột thu gọn-thu gọn (*reduce‑reduce conflicts*)
* So sánh: LL(1) so với SLR(1) so với LALR(1) so với LR(1)

### Xử lý lỗi trong Phân tích cú pháp

* Phục hồi chế độ hoảng loạn (*Panic mode recovery*)
* Phục hồi cấp độ cụm từ (*Phrase‑level recovery*)
* Luật sinh lỗi (*Error productions*)

---

## Chương 4: Dịch hướng Cú pháp (Syntax‑Directed Translation - SDT)

* Định nghĩa hướng cú pháp (*Syntax‑directed definitions - SDD*)
* Thuộc tính thừa kế (*Inherited*) so với thuộc tính tổng hợp (*synthesized attributes*)
* Đồ thị phụ thuộc và thứ tự đánh giá (sắp xếp topo - *topological sort*)
* Định nghĩa S-attributed (chỉ chứa thuộc tính tổng hợp)
* Định nghĩa L-attributed (từ trái qua phải, cho phép thuộc tính thừa kế)
* Sơ đồ dịch hướng cú pháp (*Syntax-directed translation schemes*)
* Xây dựng AST bằng cách sử dụng SDT
* Dịch các biểu thức, câu lệnh điều khiển luồng, và khai báo biến
* Kiểm tra kiểu (*Type checking*):
  * Hệ thống kiểu (tĩnh - *static* so với động - *dynamic*)
  * Sự tương đương kiểu (tên - *name* so với cấu trúc - *structural*)
  * Chuyển đổi kiểu (ép kiểu - *coercion*)
  * Lỗi ngữ nghĩa (*Semantic errors*)

---

## Chương 5: Sinh mã Trung gian (Intermediate Code Generation)

* Sự cần thiết của biểu diễn trung gian (*Intermediate Representation - IR*)
* Mã ba địa chỉ (*Three‑address code - TAC*):
  * Quadruples (bộ bốn), triples (bộ ba), indirect triples (bộ ba gián tiếp)
* Các lệnh TAC phổ biến: gán, số học, nhảy có điều kiện/không điều kiện, gọi chương trình con (thủ tục)
* Dịch các biểu thức: cây cú pháp → TAC (sử dụng các biến tạm thời)
* Dịch điều khiển luồng (*control flow*):
  * Vòng lặp If‑then‑else, while, do‑while, for
  * Mã ngắn mạch (short-circuit code) cho các toán tử logic (&&, ||)
* Dịch mảng: tính toán địa chỉ (hàng ưu tiên - *row‑major*, cột ưu tiên - *column‑major*)
* Dịch các thủ tục và lời gọi hàm
* Xử lý biểu thức boolean (backpatching – lý thuyết nâng cao cho GATE)

---

## Chương 6: Bảng Ký hiệu và Môi trường Thực thi (Symbol Table & Runtime Environment)

### Bảng ký hiệu (Symbol Table)

* Thông tin lưu trữ: tên, kiểu, phạm vi (*scope*), vị trí bộ nhớ, v.v.
* Cấu trúc dữ liệu cho bảng ký hiệu (bảng băm, danh sách liên kết, cây)
* Tầm vực (Phạm vi): phạm vi tĩnh/từ vựng (*lexical/static*) so với phạm vi động (*dynamic scoping*)
* Tầm vực lồng nhau và tổ chức bảng ký hiệu

### Môi trường thực thi (Runtime Environment)

* Sắp xếp bộ nhớ của một chương trình đang chạy:
  * Phân đoạn mã lệnh (*Code / text segment*)
  * Phân đoạn dữ liệu (*Data segment*) (đã khởi tạo, chưa khởi tạo – BSS)
  * Phân đoạn ngăn xếp (*Stack segment*)
  * Phân đoạn heap (*Heap segment*)
* Bản ghi kích hoạt (*Activation records / stack frames*):
  * Cấu trúc: địa chỉ trả về, biến cục bộ, tham số, con trỏ frame trước, liên kết tĩnh (*static link / access link*)
* Quy ước gọi hàm (*Calling conventions*): thanh ghi do bên gọi bảo lưu (*caller‑saved*) so với thanh ghi do bên được gọi bảo lưu (*callee‑saved*)
* Quản lý heap: malloc/free, cơ bản về thu gom rác (*garbage collection* như mark‑and‑sweep, đếm tham chiếu)
* Con trỏ lơ lửng (*Dangling pointers*), rò rỉ bộ nhớ (*memory leaks*) (mặt khái niệm)

---

## Chương 7: Tối ưu hóa Mã nguồn (Code Optimization)

### Các khái niệm cơ bản

* Các khối cơ bản (*Basic blocks*) và đồ thị luồng chương trình (*flow graphs*)
* Điểm dẫn đầu (*Leaders*) và phân chia thành các khối cơ bản
* Đồ thị luồng điều khiển (*Control Flow Graph - CFG*)

### Tối ưu hóa độc lập phần cứng (Machine‑Independent Optimizations)

* Tối ưu hóa cục bộ (trong phạm vi một khối cơ bản):
  * Gộp hằng số (*Constant folding*)
  * Lan truyền hằng số (*Constant propagation*)
  * Lan truyền bản sao (*Copy propagation*)
  * Loại bỏ mã chết (*Dead code elimination*)
  * Đơn giản hóa đại số (*Algebraic simplification*)
* Tối ưu hóa lỗ nhìn (*Peephole optimization*) (dựa trên cửa sổ trượt)
* Tối ưu hóa toàn cục (giữa các khối):
  * Loại bỏ biểu thức con chung (*Common subexpression elimination*)
  * Dịch chuyển mã (mã bất biến trong vòng lặp - *loop‑invariant code*)
  * Loại bỏ biến cảm ứng (*Induction variable elimination*)
  * Giảm độ mạnh toán tử (*Strength reduction* - ví dụ: `x*2` → `x<<1`)
  * Trải vòng lặp (*Loop unrolling*), hợp nhất/phân chia vòng lặp
  * Chèn nội tuyến hàm (*Function inlining*) (mặt khái niệm)

### Phân tích luồng dữ liệu (Data Flow Analysis)

* Định nghĩa đạt tới (*Reaching definitions*)
* Phân tích biến sống (*Live variable analysis*)
* Biểu thức có sẵn (*Available expressions*)
* Biểu thức rất bận (*Very busy expressions*)
* Chuỗi Use‑define (ud‑chains) và chuỗi def‑use (du‑chains)
* Khung phương trình luồng dữ liệu (forward/backward, may/must)

### Tối ưu hóa phụ thuộc phần cứng (Machine‑Dependent Optimizations)

* Phân bổ thanh ghi (*Register allocation*) (tô màu đồ thị - *graph coloring*, spilling)
* Lập lịch lệnh (*Instruction scheduling*) (cơ bản về đường ống - *pipelining*)

---

## Chương 8: Sinh mã (Code Generation)

* Các vấn đề trong sinh mã: lựa chọn lệnh, phân bổ thanh ghi, thứ tự đánh giá
* Sinh mã đơn giản từ TAC (sử dụng ngăn xếp hoặc thanh ghi)
* Phân bổ thanh ghi bằng phương pháp tô màu đồ thị – ý tưởng cơ bản
* Tối ưu hóa lỗ nhìn (*Peephole optimization*) trên mã đích
* Sinh mã cho biểu thức (sử dụng thuật toán Sethi‑Ullman – mặt khái niệm)
* Các mô hình máy đích: máy dựa trên ngăn xếp (*stack machine*) so với máy dựa trên thanh ghi (*register machine*)

---

## Chương 9: Thuật toán phân tích cú pháp chuyên sâu (Phỏng vấn)

| Loại Parser (Trình phân tích) | Kích thước bảng | Sức mạnh cú pháp | Độ quan trọng khi phỏng vấn |
|-------------|------------|-------|----------------------|
| Đệ quy đi xuống (Recursive Descent) | Không có (bằng code) | LL(1) có quay lui (hoặc bất kỳ ngữ pháp không nhập nhằng nào) | Rất cao (thực hành viết code) |
| LL(1) | Nhỏ | Phân tích từ trên xuống đơn định | Trung bình |
| SLR(1) | Trung bình | Item LR(0) + tập FOLLOW | Thấp |
| LALR(1) | Trung bình | Hợp nhất các trạng thái LR(1) | Thấp |
| LR(1) | Lớn | LR(1) đầy đủ | Thấp |

* Chuyển đổi ngữ pháp nhập nhằng thành không nhập nhằng để phân tích cú pháp
* Loại bỏ đệ quy trái và nhân tử hóa trái – từng bước một
* Xây dựng bảng CLR(1) và LALR(1) (chỉ phục vụ kỳ thi GATE)

---

## Chương 10: So sánh quan trọng và các khái niệm phục vụ Phỏng vấn

* Trình biên dịch (*Compiler*) vs Trình thông dịch (*Interpreter*) vs Trình hợp dịch (*Assembler*)
* Liên kết tĩnh (*Static linking*) vs Liên kết động (*Dynamic linking*)
* Tầm vực tĩnh (*Static scoping*) vs Tầm vực động (*Dynamic scoping*)
* Thuộc tính thừa kế (*Inherited*) vs Thuộc tính tổng hợp (*Synthesized attributes*)
* DFA vs NFA (trong ngữ cảnh phân tích từ vựng)
* Phân tích cú pháp từ trên xuống (*Top‑down*) vs từ dưới lên (*Bottom‑up parsing*)
* SLR vs LALR vs LR(1)
* Mã ba địa chỉ (*Three‑address code*) vs Cây cú pháp trừu tượng (*AST*)
* Bộ nhớ Heap vs Ngăn xếp Stack
* Gộp hằng số (*Constant folding*) vs Lan truyền hằng số (*Constant propagation*)
* Đệ quy (*Recursion*) vs Vòng lặp (*Iteration*) (trong cài đặt trình biên dịch)

---

## Chương 11: Các chủ đề Nâng cao & Khác (Miscellaneous)

* Đồ thị có hướng không chu trình (*Directed Acyclic Graphs - DAG*) để phát hiện biểu thức con chung
* Cây phân tích cú pháp (*Parse tree*) vs Cây AST vs Đồ thị DAG
* Chiến lược đánh giá ngữ pháp thuộc tính (từ trên xuống, từ dưới lên)
* Cơ chế truyền tham số (truyền bằng giá trị, tham chiếu, tên, kết quả - call by value, reference, name, result)
* Xử lý các tính năng hướng đối tượng (vtable, dynamic dispatch – cơ bản)
* Biên dịch động Just‑in‑time (JIT) (mặt khái niệm)

---

## Chương 12: Bài tập Tính toán và Giải quyết vấn đề

### Bài tập tính toán

* Tính toán tập FIRST và FOLLOW cho một ngữ pháp cho trước
* Kiểm tra xem một ngữ pháp có phải là LL(1) hay không và xây dựng bảng LL(1)
* Xây dựng các item LR(0) và xác định các xung đột SLR(1)
* Tìm số lượng biến tạm thời trong mã TAC cho một biểu thức
* Tính toán kích thước bản ghi kích hoạt (tham số + biến cục bộ + địa chỉ trả về)
* Nhận diện mã bất biến trong vòng lặp (*loop‑invariant code*)
* Xác định các khối cơ bản và đồ thị luồng
* Áp dụng lan truyền hằng số và loại bỏ mã chết

### Các câu hỏi phỏng vấn thực tế

* "Viết biểu thức chính quy để khớp với tên biến hợp lệ trong ngôn ngữ C."
* "Thiết kế một DFA chấp nhận các chuỗi có số lượng ký tự 0 và 1 bằng nhau."
* "Cài đặt một hàm để kiểm tra tính cân bằng của các dấu ngoặc sử dụng ngăn xếp."
* "Viết một bộ phân tích cú pháp dự đoán cho một ngữ pháp số học đơn giản."
* "Cho trước một cây AST, hãy sinh mã ba địa chỉ (TAC)."
* "Giải thích sơ đồ phân bổ bộ nhớ của chương trình C này: `int x; static int y; int *p = malloc(10);`"
* "Điều gì xảy ra khi bạn gọi một hàm? Hãy giải thích quá trình tạo stack frame."
* "Làm thế nào bạn có thể loại bỏ các biểu thức con chung từ một khối cơ bản?"

---

## Ủng hộ (Support)

Nếu đề cương này giúp bạn vượt qua kỳ thi hoặc đạt được công việc mơ ước, hãy cân nhắc tặng một ngôi sao ⭐ cho repo nhé. Các đóng góp, sửa lỗi và đề xuất chủ đề mới luôn được chào đón. Chúc bạn biên dịch thành công con đường sự nghiệp của mình!
