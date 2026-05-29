# Chương 14: Các chủ đề Phỏng vấn Quan trọng (Important Interview Topics)

Chương này tổng hợp và đối chiếu các so sánh cốt lõi giữa các cấu trúc dữ liệu, thuật toán và các mô hình giải quyết bài toán thường xuyên xuất hiện nhất trong các buổi phỏng vấn kỹ thuật (technical interviews). Mỗi phần sẽ làm rõ các khía cạnh đánh đổi về mặt hiệu năng (trade-offs), các trường hợp áp dụng thực tế và các câu hỏi phỏng vấn điển hình.

## 1. Mảng so với Danh sách liên kết (Array vs Linked List)

| Đặc điểm so sánh | Mảng (Array) | Danh sách liên kết (Linked List) |
|---------|-------|--------------|
| **Cơ chế lưu trữ** | Khối bộ nhớ liên tục | Phân bố bộ nhớ không liên tục, các nút kết nối với nhau qua con trỏ |
| **Truy cập phần tử theo chỉ số** | $O(1)$ | $O(n)$ (bắt buộc phải duyệt tuần tự từ đầu) |
| **Thêm/Xóa ở đầu** | $O(n)$ (phải dịch chuyển các phần tử phía sau) | $O(1)$ (chỉ cập nhật lại con trỏ đầu Head) |
| **Thêm/Xóa ở cuối** (nếu biết con trỏ cuối Tail) | $O(1)$ phân bổ (Amortized) cho mảng động | $O(1)$ nhờ duy trì con trỏ Tail |
| **Thêm/Xóa tại vị trí bất kỳ** | $O(n)$ | Tốn $O(n)$ để tìm vị trí, mất $O(1)$ để cập nhật liên kết con trỏ |
| **Chi phí bộ nhớ phụ trội** | Tối thiểu | Tốn thêm không gian lưu trữ con trỏ (8 bytes mỗi nút trên hệ thống 64-bit) |
| **Tính cục bộ của bộ đệm (Cache locality)** | Cực kỳ tốt (cho phép nạp trước dữ liệu prefetching) | Rất kém (phải truy vết con trỏ liên tục pointer chasing) |
| **Khả năng thay đổi kích thước** | Cố định hoặc tốn chi phí tăng kích thước phân bổ | Tự động mở rộng hoặc thu hẹp động một cách tự nhiên |

**Lời khuyên khi phỏng vấn**:
- Hãy sử dụng **Mảng** khi bạn cần truy cập ngẫu nhiên các phần tử với tần suất cao và đã biết trước kích thước xấp xỉ của tập dữ liệu.
- Hãy sử dụng **Danh sách liên kết** khi bạn cần thực hiện nhiều thao tác thêm/xóa phần tử tại các vị trí đã biết trước (ví dụ: cài đặt bộ nhớ đệm LRU Cache).
- **Câu hỏi điển hình**: Đảo ngược danh sách liên kết, phát hiện chu trình trên danh sách liên kết, tìm nút nằm ở giữa danh sách liên kết.

## 2. Ngăn xếp so với Hàng đợi so với Đống (Stack vs Queue vs Heap)

| Đặc điểm so sánh | Ngăn xếp (Stack - LIFO) | Hàng đợi (Queue - FIFO) | Đống / Hàng đợi ưu tiên (Heap - Priority Queue) |
|---------|--------------|--------------|------------------------|
| **Thứ tự truy cập** | Vào sau - Ra trước (Last‑in‑first‑out) | Vào trước - Ra trước (First‑in‑first‑out) | Dựa trên độ ưu tiên (nhỏ nhất hoặc lớn nhất) |
| **Các thao tác $O(1)$ điển hình** | `push`, `pop`, `top` | `enqueue`, `dequeue`, `front` | Lấy phần tử cực trị `getMin`/`getMax` là $O(1)$, thêm/xóa là $O(\log n)$ |
| **Cách thức cài đặt** | Sử dụng mảng, danh sách liên kết | Sử dụng mảng vòng, danh sách liên kết | Sử dụng đống nhị phân (biểu diễn qua mảng) |
| **Trường hợp áp dụng điển hình** | Quản lý lời gọi hàm, tính năng Undo, phân tích cú pháp | Thuật toán BFS, lập lịch tác vụ, bộ đệm dữ liệu buffering | Thuật toán Dijkstra, mã hóa Huffman, tìm phần tử lớn thứ K |

**Lời khuyên khi phỏng vấn**:
- **Ngăn xếp (Stack)**: Thường dùng giải quyết các bài toán kiểm tra dấu ngoặc hợp lệ, tìm phần tử lớn hơn tiếp theo (next greater element), tính giá trị biểu thức hậu tố.
- **Hàng đợi (Queue)**: Dùng duyệt cây theo tầng (level order traversal), tìm giá trị lớn nhất trên cửa sổ trượt (sử dụng hàng đợi hai đầu Deque).
- **Đống (Heap)**: Dùng để gộp K danh sách đã sắp xếp, tìm giá trị trung vị từ một luồng dữ liệu liên tục.

## 3. Đệ quy so với Vòng lặp (Recursion vs Iteration)

| Khía cạnh đối chiếu | Đệ quy (Recursion) | Vòng lặp (Iteration) |
|--------|-----------|-----------|
| **Độ sáng sủa của mã nguồn** | Cực kỳ ngắn gọn và tự nhiên đối với các bài toán có tính chất đệ quy (duyệt cây, chia để trị) | Viết nhiều mã nguồn hơn nhưng thường dễ theo dõi dòng chạy đối với các vòng lặp đơn giản |
| **Hiệu năng thực thi** | Tốn chi phí quản lý lời gọi hàm (gọi hàm hệ thống) | Không tốn chi phí quản lý lời gọi hàm |
| **Độ phức tạp bộ nhớ** | Tốn không gian ngăn xếp hệ thống tỷ lệ với độ sâu đệ quy $O(\text{depth})$ | Thường chỉ tốn thêm không gian bộ nhớ hằng số $O(1)$ |
| **Nguy cơ tiềm ẩn** | Dễ gây ra lỗi tràn bộ nhớ ngăn xếp (Stack Overflow) nếu đệ quy quá sâu | Hoàn toàn không lo tràn ngăn xếp hệ thống |
| **Đệ quy đuôi (Tail recursion)** | Có thể được trình biên dịch tối ưu hóa thành vòng lặp (không bảo đảm tuyệt đối) | Không áp dụng |

**Khi nào nên dùng Đệ quy**:
- Duyệt cây hoặc đồ thị (DFS).
- Các thuật toán chia để trị (sắp xếp trộn Merge Sort, sắp xếp nhanh Quick Sort).
- Các thuật toán quay lui Backtracking (bài toán N-Queens, sinh hoán vị).

**Khi nào nên dùng Vòng lặp**:
- Khi độ sâu đệ quy quá lớn ($n > 10^5$).
- Các vòng lặp đòi hỏi tối ưu hiệu năng cực hạn.
- Khi yêu cầu bài toán bắt buộc sử dụng không gian bộ nhớ hằng số.

**Lời khuyên khi phỏng vấn**: Người phỏng vấn rất hay yêu cầu bạn cài đặt cả hai cách giải (ví dụ: tính số Fibonacci, tìm kiếm nhị phân, duyệt cây). Bạn cần thành thạo kỹ thuật chuyển đổi từ đệ quy sang vòng lặp bằng cách sử dụng một cấu trúc ngăn xếp (Stack) mô phỏng tường minh.

## 4. DFS so với BFS (DFS vs BFS)

| Đặc điểm so sánh | Tìm kiếm theo chiều sâu (DFS) | Tìm kiếm theo chiều rộng (BFS) |
|---------|-----|-----|
| **Cấu trúc dữ liệu hỗ trợ** | Ngăn xếp (Stack - đệ quy hệ thống hoặc ngăn xếp tường minh) | Hàng đợi (Queue) |
| **Độ phức tạp bộ nhớ** | $O(\text{depth})$ – có thể lên tới $O(n)$ trong trường hợp cây lệch (skewed tree) | $O(\text{width})$ – thường tốn bộ nhớ hơn đối với các đồ thị có bề ngang rất rộng |
| **Đường đi ngắn nhất (không trọng số)** | Chỉ tìm thấy một đường đi bất kỳ chứ không bảo đảm ngắn nhất | Chắc chắn tìm ra đường đi ngắn nhất (ít số cạnh nhất) |
| **Phát hiện chu trình** | Cực kỳ dễ dàng (dựa vào đống đệ quy) | Khó hơn, đòi hỏi cơ chế theo dõi bổ sung |
| **Sắp xếp Topo** | Triển khai tự nhiên dựa trên DFS (hậu thứ postorder) | Sử dụng thuật toán Kahn dựa trên BFS |
| **Trường hợp áp dụng tốt nhất** | Tìm các thành phần liên thông, giải mê cung, giải quyết các ràng buộc | Tìm đường đi ngắn nhất, thu thập thông tin web (web crawler), duyệt cây theo tầng |

**Lời khuyên khi phỏng vấn**:
- Hãy dùng **BFS** cho các bài toán tìm đường đi ngắn nhất trên lưới ô vuông (bài toán chuỗi biến đổi từ word ladder, khoảng cách ngắn nhất trên lưới số).
- Hãy dùng **DFS** cho các bài toán cần tìm kiếm vét cạn tất cả các cấu hình khả dĩ (tìm hoán vị, tập con) hoặc khi đồ thị có chiều sâu lớn nhưng bề ngang hẹp.
- Hãy luôn sẵn sàng viết mã nguồn cho cả hai thuật toán duyệt này trên cả đối tượng cây lẫn đồ thị.

## 5. Bảng băm so với Cây tìm kiếm (Hash Map vs Tree Map)

| Đặc điểm so sánh | Bảng băm (Hash Map - `unordered_map`) | Cây tìm kiếm (Tree Map - `map`) |
|---------|--------------------------|----------------|
| **Cấu trúc bên dưới** | Bảng băm (Hash Table) | Cây tìm kiếm nhị phân cân bằng (ví dụ: Cây Đỏ-Đen) |
| **Thời gian trung bình (thêm/tìm/xóa)** | $O(1)$ | $O(\log n)$ |
| **Thời gian trong trường hợp xấu nhất** | $O(n)$ (khi xảy ra nhiều xung đột mã băm) | Luôn bảo đảm tuyệt đối là $O(\log n)$ |
| **Thứ tự các phần tử** | Hoàn toàn không sắp thứ tự | Luôn được sắp xếp tăng dần theo khóa (key) |
| **Chi phí bộ nhớ phụ trội** | Phụ thuộc vào hệ số tải (load factor) của bảng băm | Chi phí lưu trữ các con trỏ nhánh trái, nhánh phải, thông tin màu nút |
| **Trường hợp áp dụng** | Tra cứu nhanh, đếm tần suất xuất hiện | Cần duyệt phần tử theo thứ tự, truy vấn theo đoạn (range queries), tìm phần tử lớn nhất/nhỏ nhất kề cạnh |

**Lời khuyên khi phỏng vấn**: Trong ngôn ngữ C++, `std::unordered_map` đại diện cho bảng băm, còn `std::map` đại diện cho cây tìm kiếm. Bạn cần hiểu rõ khi nào nên ưu tiên dùng loại nào. Ví dụ: đối với truy vấn khoảng (đếm số lượng khóa nằm trong đoạn từ $L$ đến $R$), cấu trúc Tree Map là lựa chọn bắt buộc để đạt hiệu năng tối ưu.

## 6. So sánh các thuật toán sắp xếp (Comparison of Sorting Algorithms)

| Thuật toán | Trường hợp tốt nhất | Trường hợp trung bình | Trường hợp xấu nhất | Bộ nhớ phụ trội | Ổn định (Stable) | Tại chỗ (In‑place) | Thích ứng (Adaptive) |
|-----------|------|---------|-------|-------|--------|----------|----------|
| **Sắp xếp nổi bọt (Bubble)** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Có | Có | Có |
| **Sắp xếp chọn (Selection)** | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Không | Có | Không |
| **Sắp xếp chèn (Insertion)** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Có | Có | Có |
| **Sắp xếp trộn (Merge)** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Có | Không | Không |
| **Sắp xếp nhanh (Quick)** | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | Không | Có | Không |
| **Sắp xếp vun đống (Heap)** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | Không | Có | Không |
| **Sắp xếp đếm (Counting)** | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | $O(k)$ | Có | Không | Không |
| **Sắp xếp cơ số (Radix)** | $O(n \cdot k)$ | $O(n \cdot k)$ | $O(n \cdot k)$ | $O(n+k)$ | Có | Không | Không |

- **Tính ổn định (Stability)**: Bảo đảm giữ nguyên thứ tự ban đầu của các phần tử có giá trị bằng nhau. Điều này rất quan trọng khi ta cần sắp xếp một danh sách theo nhiều khóa nối tiếp nhau (ví dụ: sắp xếp danh sách theo tên, sau đó sắp xếp tiếp theo tuổi).

**Lời khuyên khi phỏng vấn**:
- Thuật toán **Quick Sort** thường cho tốc độ thực tế nhanh nhất đối với mảng (nhờ tính cục bộ của bộ đệm cache cực tốt).
- Thuật toán **Merge Sort** có tính ổn định cao và thường được chọn để sắp xếp danh sách liên kết (vì không tốn không gian mảng phụ trợ) hoặc áp dụng trong sắp xếp ngoài (external sorting).
- Hãy dùng **Counting Sort** hoặc **Radix Sort** khi miền giá trị số nguyên $k$ của các phần tử được giới hạn nhỏ (ví dụ: sắp xếp 1 triệu dữ liệu tuổi từ 0 đến 120).

## 7. Danh sách liên kết đơn so với Danh sách liên kết kép (Singly vs Doubly Linked List)

| Đặc điểm so sánh | Danh sách liên kết đơn | Danh sách liên kết kép |
|---------|--------------------|--------------------|
| **Cấu trúc một nút** | `data` + con trỏ `next` | `data` + con trỏ `prev` + con trỏ `next` |
| **Kích thước bộ nhớ mỗi nút** | Thấp hơn | Cao hơn (phải lưu trữ thêm 1 con trỏ) |
| **Duyệt ngược chiều** | Không thể thực hiện (nếu không đảo ngược danh sách mất $O(n)$) | $O(1)$ duyệt ngược rất dễ dàng |
| **Xóa một nút cho trước** (không có con trỏ nút trước) | Tốn $O(n)$ để tìm kiếm nút phía trước | $O(1)$ cực nhanh nhờ con trỏ `prev` có sẵn |
| **Thêm phần tử vào trước một nút** | $O(n)$ | $O(1)$ |
| **Trường hợp áp dụng điển hình** | Cài đặt ngăn xếp, hàng đợi, danh sách kề đồ thị | Cài đặt LRU cache, tính năng Undo/Redo, lịch sử trình duyệt |

**Lời khuyên khi phỏng vấn**: Nhiều bài toán (như kiểm tra danh sách đối xứng, đảo ngược theo nhóm) sẽ đơn giản hơn rất nhiều nếu dùng danh sách liên kết kép, tuy nhiên đề bài phỏng vấn thường bắt buộc bạn phải giải quyết trên danh sách liên kết đơn để kiểm tra kỹ năng lập trình và làm chủ con trỏ của ứng viên.

## 8. Cây tìm kiếm nhị phân so với Bảng băm (BST vs Hash Table)

| Đặc điểm so sánh | Cây tìm kiếm nhị phân cân bằng (BST) | Bảng băm (Hash Table) |
|---------|----------------|------------|
| **Trung bình (tìm/thêm/xóa)** | $O(\log n)$ | $O(1)$ |
| **Worst-case (lệch / xung đột băm)** | $O(n)$ | $O(n)$ |
| **Thứ tự khóa** | Các khóa luôn nằm trong trạng thái được sắp xếp | Hoàn toàn không sắp thứ tự |
| **Truy vấn khoảng** (tìm phần tử từ $L$ đến $R$) | $O(\log n + k)$ với $k$ là số phần tử nằm trong khoảng | Không thể thực hiện hiệu quả (bắt buộc phải duyệt qua toàn bộ) |
| **Tìm phần tử đứng trước / đứng sau** | $O(\log n)$ | Không hỗ trợ |
| **Bộ nhớ sử dụng** | Lưu trữ các con trỏ quản lý nút | Chi phí dự phòng cho hệ số tải bảng băm |

**Lời khuyên khi phỏng vấn**: Hãy dùng **Bảng băm** khi cần tra cứu chính xác phần tử (như từ điển). Hãy dùng **Cây tìm kiếm nhị phân** khi thứ tự của các khóa là quan trọng (như truy vấn khoảng giá trị, tìm kiếm giá trị tiệm cận gần nhất). Đối với C++, ta so sánh giữa `std::set` (tập hợp có sắp thứ tự) với `std::unordered_set` (tập hợp băm).

## 9. Khi nào nên dùng: Chia để trị vs Quy hoạch động vs Tham lam vs Quay lui

| Phương pháp thiết kế | Trường hợp áp dụng thích hợp nhất | Ví dụ điển hình |
|----------|-------------|---------|
| **Chia để trị (Divide & Conquer)** | Các bài toán con hoàn toàn độc lập và không chồng chéo lên nhau | Thuật toán Merge Sort, tìm cặp điểm gần nhất, nhân nhanh Karatsuba |
| **Quy hoạch động (Dynamic Programming)** | Các bài toán con trùng lặp nhiều lần + có cấu trúc con tối ưu | Bài toán cái túi, dãy con chung dài nhất LCS, khoảng cách Levenshtein |
| **Thuật toán Tham lam (Greedy)** | Lựa chọn tối ưu cục bộ dẫn đến lời giải tối ưu toàn cục | Lựa chọn hoạt động, cái túi dạng phân số, thuật toán Dijkstra |
| **Quay lui (Backtracking)** | Cần duyệt và thử nghiệm qua tất cả các cấu hình khả dĩ | Bài toán xếp quân hậu N-Queens, giải Sudoku, sinh hoán vị |

**Quy trình đưa ra quyết định**:
1. Có thể chia bài toán lớn thành các bài toán con hoàn toàn độc lập không? $\to$ Chọn **Chia để trị**.
2. Các bài toán con có bị trùng lặp tính toán lại nhiều lần không? $\to$ Chọn **Quy hoạch động** (đệ quy có nhớ hoặc lập bảng).
3. Lựa chọn tối ưu cục bộ hiện tại có luôn dẫn đến tối ưu toàn cục không? $\to$ Chọn **Tham lam** (hãy thử tìm ví dụ phản chứng nhỏ).
4. Cần phải duyệt toàn bộ tất cả các cấu hình để tìm đáp án thỏa mãn ràng buộc? $\to$ Chọn **Quay lui**.

**Lời khuyên khi phỏng vấn**: Đối với các bài toán tối ưu hóa (tìm cực đại/cực tiểu), nếu không tìm được ví dụ phản chứng cho giải pháp tham lam thì hãy triển khai tham lam; ngược lại, hãy thiết kế giải pháp Quy hoạch động. Nếu đề bài yêu cầu tìm "tất cả các cách xếp" hoặc "đếm toàn bộ cấu hình", hãy nghĩ ngay tới Quay lui.

## 10. Ứng dụng của Cấu trúc DSU (Union-Find)

**Khái niệm**: DSU là cấu trúc dữ liệu theo dõi việc phân chia các phần tử thành các tập hợp rời nhau. DSU hỗ trợ hai thao tác: `find` (xác định phần tử thuộc tập hợp nào) và `union` (gộp hai tập hợp lại với nhau) với độ phức tạp tiệm cận $O(1)$ nhờ áp dụng hai kỹ thuật tối ưu hóa: nén đường đi (path compression) và gộp theo cấp/kích thước (union by rank/size).

**Các ứng dụng thực tế phổ biến**:
- **Thuật toán Kruskal** tìm cây khung tối tiểu – dùng để phát hiện chu trình khi thêm các cạnh vào cây khung.
- **Tính liên thông động** – xác định xem hai đỉnh có liên thông với nhau trong đồ thị vô hướng sau khi liên tục bổ sung thêm các cạnh nối hay không.
- **Đếm số lượng thành phần liên thông** trong một đồ thị vô hướng.
- **DSU trên lưới 2 chiều** – liên kết các ô kề cạnh nhau (ví dụ: đếm số lượng đảo, tìm các vùng liên thông trên ảnh).
- **Các bài toán quan hệ tương đương** – ví dụ: tìm nhóm bạn chung trên mạng xã hội, gom nhóm các phần tử có chung tính chất.

**Ví dụ mã nguồn cài đặt DSU cơ bản**:

```cpp
int find(vector<int>& parent, int x) {
    if (parent[x] != x) parent[x] = find(parent, parent[x]); // Nén đường đi (Path compression)
    return parent[x];
}
void unite(vector<int>& parent, vector<int>& rank, int x, int y) {
    int rx = find(parent, x), ry = find(parent, y);
    if (rx == ry) return;
    if (rank[rx] < rank[ry]) parent[rx] = ry; // Gộp theo cấp (Union by rank)
    else if (rank[rx] > rank[ry]) parent[ry] = rx;
    else { parent[ry] = rx; rank[rx]++; }
}
```

**Lời khuyên khi phỏng vấn**: Hãy nắm chắc hai kỹ thuật tối ưu hóa cực kỳ quan trọng là nén đường đi và gộp theo cấp/kích thước. Hãy luyện tập để có thể tự tay viết cấu trúc DSU một cách hoàn chỉnh từ đầu.

## 11. Cây Trie so với Bảng băm trong việc tìm kiếm xâu ký tự

| Đặc điểm so sánh | Cây Trie | Bảng băm (Hash Table) |
|---------|------|-------------|
| **Tìm kiếm chính xác xâu** | $O(L)$ với $L$ là độ dài xâu ký tự | Trung bình mất $O(L)$ (để tính toán mã băm của xâu) |
| **Tìm kiếm theo tiền tố / gợi ý** | Chỉ tốn $O(L)$ duyệt theo các nhánh trên cây | Không hỗ trợ hiệu quả (bắt buộc phải quét qua tất cả các xâu) |
| **Dung lượng bộ nhớ tiêu thụ** | Có thể rất lớn (do tạo ra rất nhiều nút trên cây) | Tiết kiệm bộ nhớ hơn khi số lượng từ không quá nhiều |
| **Thao tác xóa từ** | $O(L)$ | Trung bình mất $O(1)$ |
| **Sự phụ trội về không gian** | Tốn chi phí lưu trữ các mảng con trỏ cho các ký tự tại mỗi nút | Chi phí dự phòng hệ số tải bảng băm |
| **Ứng dụng thực tế** | Từ điển có hỗ trợ gợi ý hoàn thành từ, định tuyến IP | Tra cứu chính xác từ, đếm tần suất xuất hiện từ |

**Khi nào nên dùng Cây Trie**:
- Ứng dụng tự động hoàn thành từ (auto-complete / gợi ý tìm kiếm).
- Kiểm tra chính tả (tìm các từ có khoảng cách biến đổi nhỏ).
- Tìm tiền tố chung dài nhất (longest common prefix).
- Bài toán tách từ (word break) kết hợp Quy hoạch động và cây Trie.

**Khi nào nên dùng Bảng băm**:
- Kiểm tra đơn giản sự tồn tại của từ.
- Đếm tần suất xuất hiện của từ.
- Hoàn toàn không có các yêu cầu liên quan đến tiền tố (prefix).

**Lời khuyên khi phỏng vấn**: Có rất nhiều bài toán giải quyết được bằng cả hai cấu trúc (ví dụ: tìm kiếm từ, tách từ), nhưng cây Trie mang lại hiệu năng tìm kiếm tiền tố $O(L)$ vượt trội và luôn là giải pháp được người phỏng vấn kỳ vọng đối với các bài toán có tính chất so khớp tiền tố xâu.

## 12. Kỹ thuật hai con trỏ so với Cửa sổ trượt (Two‑Pointer vs Sliding Window)

| Khía cạnh đối chiếu | Kỹ thuật Hai con trỏ (Two‑Pointer) | Cửa sổ trượt (Sliding Window) |
|---------|-------------|----------------|
| **Cơ chế hoạt động điển hình** | Hai con trỏ di chuyển ngược chiều nhau (từ hai đầu vào giữa) hoặc di chuyển cùng chiều với tốc độ khác nhau | Sử dụng hai con trỏ trái và phải để duy trì ranh giới của một cửa sổ liên tục, cửa sổ sẽ tự động co giãn |
| **Trường hợp áp dụng** | Kiểm tra xâu đối xứng, tìm cặp phần tử có tổng thỏa mãn trên mảng đã sắp xếp, loại bỏ phần tử trùng lặp, trộn hai mảng đã sắp xếp | Các bài toán tìm mảng con/xâu con liên tiếp thỏa mãn một số điều kiện ràng buộc nhất định (như tổng giá trị, số ký tự phân biệt) |
| **Kích thước cửa sổ** | Cố định hoặc thay đổi tùy ý (thường không yêu cầu quản lý chặt chẽ toàn bộ các phần tử ở giữa) | Kích thước thay đổi hoặc cố định, nhưng luôn đại diện cho một phân đoạn liên tiếp |
| **Điều kiện dịch chuyển** | Thường dựa trên việc so sánh giá trị tại hai đầu con trỏ | Thường dựa trên tính khả thi của ràng buộc trên cửa sổ (ví dụ: tổng $\le K$) |
| **Độ phức tạp thời gian** | Luôn đạt $O(n)$ | Luôn đạt $O(n)$ |

**Các ví dụ điển hình**:
- **Hai con trỏ ngược chiều**: `isPalindrome`, tìm cặp số có tổng bằng $S$ trong mảng đã sắp xếp.
- **Hai con trỏ cùng chiều**: `removeDuplicates`, tìm nút ở giữa danh sách liên kết.
- **Cửa sổ trượt kích thước cố định**: Tìm tổng lớn nhất của mảng con có kích thước bằng $K$.
- **Cửa sổ trượt kích thước thay đổi**: Tìm xâu con dài nhất không chứa ký tự lặp lại.

**Lời khuyên khi phỏng vấn**: Cửa sổ trượt thực chất là một dạng biến thể đặc biệt của kỹ thuật hai con trỏ dùng để quản lý một phân đoạn liên tiếp. Cách nhận diện nhanh chóng: nếu đề bài yêu cầu tối ưu trên một **phân đoạn liên tiếp** thỏa mãn ràng buộc $\to$ hãy chọn **Cửa sổ trượt**; nếu thứ tự sắp xếp của các phần tử là quan trọng (ví dụ: so khớp từng cặp số, đối xứng) $\to$ hãy chọn **Hai con trỏ**.

## Bảng tóm tắt so sánh nhanh quyết định lựa chọn

| Cặp so sánh | Yếu tố quyết định lựa chọn cấu trúc / thuật toán tối ưu |
|------------|-----------------|
| **Array vs Linked List** | Cần truy cập ngẫu nhiên nhanh (Array) vs cần thêm/xóa liên tục tại các vị trí đã biết (Linked List). |
| **Stack vs Queue vs Heap** | Thứ tự truy cập dữ liệu: LIFO (Stack), FIFO (Queue), hay dựa vào độ ưu tiên lớn nhất/nhỏ nhất (Heap). |
| **Recursion vs Iteration** | Độ sâu của các lời gọi hàm, độ sáng sủa dễ đọc của mã nguồn, giới hạn dung lượng ngăn xếp hệ thống. |
| **DFS vs BFS** | Cần tìm đường đi ngắn nhất trên đồ thị không trọng số (BFS) vs cần tìm kiếm vét cạn exhaustive search (DFS). |
| **Hash Map vs Tree Map** | Có cần duy trì thứ tự các khóa hay không (Tree Map cần sắp xếp, Hash Map cho tốc độ trung bình nhanh hơn). |
| **Sorting Algorithms** | Phụ thuộc vào kích thước dữ liệu đầu vào, yêu cầu về tính ổn định stable, và yêu cầu sắp xếp tại chỗ in-place. |
| **Singly vs Doubly List** | Có cần duyệt danh sách theo cả hai chiều ngược xuôi hay không / cần xóa nút trong thời gian $O(1)$ hay không. |
| **BST vs Hash Table** | Cần thực hiện các truy vấn khoảng range queries và duy trì thứ tự (BST) vs chỉ tra cứu chính xác (Hash Table). |
| **DP / Greedy / Backtracking / D&C** | Khảo sát tính chồng chéo của các bài toán con, cấu trúc con tối ưu, và thuộc tính lựa chọn tham lam. |
| **Union‑Find (DSU)** | Giải quyết các bài toán về liên thông động, phát hiện chu trình đồ thị (trong thuật toán Kruskal). |
| **Trie vs Hash Table** | Cần giải quyết các truy vấn liên quan tới so khớp tiền tố gợi ý (Trie) vs so khớp chính xác từ (Hash Table). |
| **Two-pointer vs Sliding Window** | Ràng buộc nằm trên một phân đoạn liên tiếp (Sliding Window) vs so khớp từng cặp phần tử bất kỳ (Two-pointer). |

Hãy thường xuyên xem lại bảng so sánh này trước các buổi phỏng vấn kỹ thuật để nhanh chóng củng cố các tư duy đánh đổi hiệu năng và nhận diện chính xác các mẫu thiết kế thuật toán tối ưu nhất.
