# CẤU TRÚC DỮ LIỆU & GIẢI THUẬT (DATA STRUCTURES & ALGORITHMS - DSA)

## Chương 1: Giới thiệu về Cấu trúc dữ liệu và Giải thuật (Introduction to DSA)

* Cấu trúc dữ liệu và Giải thuật là gì?
* Tại sao DSA lại quan trọng trong phát triển phần mềm và các cuộc phỏng vấn tuyển dụng?
* Hiệu năng giải thuật: Độ phức tạp thời gian (Time complexity) và Độ phức tạp không gian (Space complexity)
* Các ký pháp tiệm cận (Asymptotic notations): Big-O, Big-Omega ($\Omega$), Big-Theta ($\Theta$)
* Các lớp độ phức tạp phổ biến: $O(1)$, $O(\log n)$, $O(n)$, $O(n \log n)$, $O(n^2)$, $O(2^n)$, $O(n!)$

---

## Chương 2: Toán học nền tảng & Các kiến thức bổ trợ (Mathematics & Prerequisites)

* Khái niệm cơ bản về Đệ quy (Recursion): Điều kiện dừng (Base case), bước đệ quy (recursive case), ngăn xếp gọi hàm (call stack)
* Nguyên lý của Thuật toán quay lui (Backtracking)
* Kiến thức toán học nền tảng:
  * Lô-ga-rít (Logarithms) và lũy thừa (exponents)
  * Tổng (Summations) và chuỗi cấp số (series)
  * Chỉnh hợp (Permutations) và tổ hợp (combinations)
  * Số học mô-đun (Modular arithmetic)
  * Số nguyên tố (Prime numbers) và ước chung lớn nhất UCLN (GCD)
* Xử lý bit (Bit manipulation):
  * Các toán tử thao tác bit (Bitwise operators): AND, OR, XOR, NOT, dịch bit (shifts)
  * Các thủ thuật bit phổ biến: kiểm tra lũy thừa của 2, đảo ngược trạng thái bit (toggling bits), đếm số bit 1 (counting set bits)

---

## Chương 3: Mảng và Chuỗi ký tự (Arrays and Strings)

### Mảng (Arrays)

* Mảng một chiều (One-dimensional) và nhiều chiều (multi-dimensional)
* Mảng động (Dynamic arrays - ví dụ: ArrayList, Vector)
* Các thao tác cơ bản: duyệt (traversal), chèn (insertion), xóa (deletion), tìm kiếm (searching)
* Mảng cộng dồn / mảng tiền tố (Prefix sum array)
* Mảng hiệu (Difference array)

### Kỹ thuật Cửa sổ trượt (Sliding Window Technique)

* Cửa sổ có kích thước cố định (Fixed-size window)
* Cửa sổ có kích thước thay đổi (Variable-size window)

### Kỹ thuật Hai con trỏ (Two Pointers Technique)

* Di chuyển ngược chiều (Đối nhau - Opposite direction, ví dụ: tổng cặp số, kiểm tra chuỗi đối xứng palindrome)
* Di chuyển cùng chiều (Song song - Same direction, ví dụ: xóa phần tử trùng lặp, gộp hai mảng đã sắp xếp)

### Chuỗi ký tự (Strings)

* Tính bất biến của chuỗi (String immutability) và lớp tối ưu xây dựng chuỗi (StringBuilders/StringBuffers)
* Các thuật toán so khớp mẫu (Pattern matching algorithms):
  * Tìm kiếm thô sơ (Naive search)
  * Thuật toán KMP (Knuth-Morris-Pratt)
  * Thuật toán Rabin-Karp (sử dụng băm cuốn - rolling hash)
* Các bài toán về chuỗi hoán vị (Anagram) và chuỗi đối xứng (Palindrome)

### Các bài toán kinh điển (Important Problems)

* Thuật toán Kadane (Tìm phân mảnh con có tổng lớn nhất - maximum subarray sum)
* Bài toán lá cờ ba màu của Hà Lan (Dutch National Flag problem - sắp xếp các số 0, 1, 2)
* Bài toán hứng nước mưa (Trapping rainwater)
* Bài toán chứa nhiều nước nhất (Container with most water)

---

## Chương 4: Danh sách liên kết (Linked Lists)

* Danh sách liên kết đơn (Singly Linked List)
* Danh sách liên kết kép (Doubly Linked List)
* Danh sách liên kết vòng (Circular Linked List)
* Các thao tác cơ bản: chèn (insertion), xóa (deletion), duyệt (traversal), tìm kiếm (search)
* Kỹ thuật hai con trỏ trên danh sách liên kết:
  * Tìm phần tử trung tâm (middle) của danh sách liên kết
  * Phát hiện chu kỳ (Floyd’s cycle detection - thuật toán rùa và thỏ)
  * Tìm điểm bắt đầu của chu kỳ
* Đảo ngược danh sách liên kết (Reversal):
  * Đảo ngược bằng vòng lặp (Iterative reversal)
  * Đảo ngược bằng đệ quy (Recursive reversal)
  * Đảo ngược danh sách theo từng nhóm $k$ phần tử
* Các bài toán gộp và giao nhau (Merge and intersection):
  * Gộp hai danh sách đã sắp xếp
  * Tìm giao điểm của hai danh sách liên kết
* Sao chép danh sách liên kết có con trỏ ngẫu nhiên (random pointer)

---

## Chương 5: Ngăn xếp và Hàng đợi (Stacks and Queues)

### Ngăn xếp (Stack)

* Nguyên lý vào sau ra trước LIFO (Last-In-First-Out)
* Cài đặt ngăn xếp bằng mảng (Array-based implementation)
* Cài đặt ngăn xếp bằng danh sách liên kết (Linked-list-based implementation)
* Các ứng dụng thực tế:
  * Kiểm tra dấu ngoặc hợp lệ (Balanced parentheses)
  * Đánh giá biểu thức toán học (trung tố - infix, hậu tố - postfix, tiền tố - prefix)
  * Tìm phần tử lớn hơn tiếp theo (Next Greater Element)
  * Tìm hình chữ nhật lớn nhất trong biểu đồ cột (Largest rectangle in histogram)
  * Ngăn xếp cực tiểu (Min stack - hỗ trợ lấy giá trị nhỏ nhất trong $O(1)$)

### Hàng đợi (Queue)

* Nguyên lý vào trước ra trước FIFO (First-In-First-Out)
* Cài đặt bằng mảng (Hàng đợi vòng - circular queue) và danh sách liên kết
* Hàng đợi hai đầu (Deque - Double-ended queue)
* Các ứng dụng thực tế:
  * Tìm phần tử lớn nhất trong cửa sổ trượt (Sliding window maximum)
  * Duyệt đồ thị theo chiều rộng BFS (Breadth-First Search)

### Hàng đợi ưu tiên / Đống (Priority Queue / Heap)

* Đống cực tiểu (Min-heap) và Đống cực đại (Max-heap)
* Thao tác vun đống (Heapify) và các thao tác trên Heap (chèn, trích xuất min/max, giảm giá trị khóa decrease key)
* Các ứng dụng thực tế:
  * Tìm phần tử lớn thứ $k$ / nhỏ thứ $k$
  * Gộp $K$ danh sách liên kết đã sắp xếp
  * Tối ưu hóa thuật toán tìm đường đi ngắn nhất Dijkstra

---

## Chương 6: Đệ quy và Quay lui (Recursion and Backtracking)

* Nguyên lý đệ quy nền tảng: điều kiện dừng (base case), công thức truy hồi (recurrence relation)
* Đệ quy đuôi (Tail recursion) so với đệ quy phi đuôi (non-tail recursion)
* Đệ quy so với Vòng lặp (Recursion vs Iteration)

### Thuật toán quay lui (Backtracking)

* Cây không gian trạng thái (State space tree)
* Các kỹ thuật nhánh cận / cắt tỉa (Pruning techniques)
* Các bài toán kinh điển:
  * Bài toán Tám quân hậu (N-Queens)
  * Giải câu đố Sudoku (Sudoku solver)
  * Chuột trong mê cung (Rat in a maze)
  * Sinh tất cả các tập con / Tập lũy thừa (Generate all subsets / powerset)
  * Sinh các hoán vị (Generate permutations)
  * Tìm tổ hợp có tổng bằng $K$ (Combination sum)

---

## Chương 7: Chia để trị (Divide and Conquer)

* **Chia (Divide), Trị (Conquer), Kết hợp (Combine)** – Mô hình ba bước kinh điển
* Công thức truy hồi cho Chia để trị (D&C): $T(n) = aT(n/b) + O(n^d)$
* Mối quan hệ với đệ quy (đệ quy là cơ chế thực thi, Chia để trị là một chiến lược thiết kế giải thuật)

### Các thuật toán Chia để trị kinh điển (Classic D&C Algorithms)

* Tìm kiếm nhị phân (Binary search) với độ phức tạp $T(n) = T(n/2) + O(1)$
* Sắp xếp trộn (Merge sort) với độ phức tạp $T(n) = 2T(n/2) + O(n)$
* Sắp xếp nhanh (Quick sort) dưới góc nhìn lý thuyết (chọn phần tử chốt pivot, phân hoạch partition, đệ quy)
* Tìm phân mảnh con có tổng lớn nhất bằng Chia để trị (Maximum subarray sum - giải pháp thay thế Kadane)
* Đếm số cặp nghịch thế (Counting inversions - sử dụng biến thể của sắp xếp trộn)
* Thuật toán Karatsuba nhân số lớn siêu tốc ($O(n^{1.585})$)
* Tìm cặp điểm gần nhau nhất trong không gian 2D ($O(n \log n)$ - lý thuyết)
* Tính lũy thừa nhanh của một số (Fast exponentiation)

### Chia để trị so với các mô hình thiết kế khác

* Chia để trị so với Quy hoạch động (Bài toán con không chồng lấn - non-overlapping subproblems vs Bài toán con chồng lấn - overlapping subproblems)
* Chia để trị so với Giải thuật tham lam (Greedy)

---

## Chương 8: Sắp xếp và Tìm kiếm (Sorting and Searching)

### Các thuật toán sắp xếp (Sorting Algorithms)

| Thuật toán | Trường hợp tốt nhất | Trường hợp trung bình | Trường hợp xấu nhất | Không gian bổ trợ | Tính ổn định |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Sắp xếp nổi bọt** (Bubble Sort) | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Có |
| **Sắp xếp chọn** (Selection Sort) | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Không |
| **Sắp xếp chèn** (Insertion Sort) | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Có |
| **Sắp xếp trộn** (Merge Sort) | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Có |
| **Sắp xếp nhanh** (Quick Sort) | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | Không |
| **Sắp xếp đống** (Heap Sort) | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | Không |
| **Sắp xếp đếm** (Counting Sort) | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | $O(k)$ | Có |
| **Sắp xếp theo cơ số** (Radix Sort) | $O(nk)$ | $O(nk)$ | $O(nk)$ | $O(n+k)$ | Có |

* So sánh giữa sắp xếp dựa trên so sánh (Comparison-based) và không dựa trên so sánh (Non-comparison)
* Sắp xếp tại chỗ (In-place) so với sắp xếp cần bộ nhớ phụ ngoài (Out-of-place)
* Sắp xếp thích ứng (Adaptive sorts)

### Tìm kiếm (Searching)

* Tìm kiếm tuyến tính (Linear search)
* Tìm kiếm nhị phân (Binary search - phiên bản lặp và đệ quy)
* Các biến thể nâng cao:
  * Tìm vị trí xuất hiện đầu tiên / cuối cùng của phần tử
  * Tìm kiếm trên mảng xoay vòng đã sắp xếp (rotated sorted array)
  * Tìm phần tử đỉnh (peak element)
* Tìm kiếm tam phân (Ternary search)

---

## Chương 9: Bảng băm (Hashing)

* Hàm băm (Hash functions)
* Giải quyết xung đột băm (Collision resolution):
  * Phương pháp kết xích (Chaining - sử dụng danh sách liên kết)
  * Phương pháp địa chỉ mở (Open addressing - dò tuyến tính linear probing, dò bậc hai quadratic probing, băm kép double hashing)
* Hệ số tải (Load factor) và kỹ thuật tái băm (Rehashing)
* Thiết kế và cài đặt cấu trúc Hash Map và Hash Set
* Các ứng dụng thực tế:
  * Bài toán tìm cặp số có tổng bằng $K$ (Two-sum problem)
  * Tìm mảng con có tổng bằng $K$ (Subarray sum equals K)
  * Tìm chuỗi số liên tục dài nhất (Longest consecutive sequence)
  * Gom nhóm các chuỗi hoán vị (Group anagrams)
  * Cơ chế bộ đệm LRU Cache (sử dụng Hash Map kết hợp Danh sách liên kết kép)

---

## Chương 10: Cây (Trees)

### Cây nhị phân (Binary Trees)

* Các thuật ngữ cơ bản: gốc (root), nút cha (parent), nút con (child), lá (leaf), chiều cao (height), độ sâu (depth)
* Các phương pháp duyệt cây nhị phân (Duyệt theo chiều sâu DFS và theo chiều rộng BFS):
  * Duyệt Trung thứ tự (Inorder), Tiền thứ tự (Preorder), Hậu thứ tự (Postorder) - phiên bản đệ quy và lặp
  * Duyệt theo mức (Level order traversal)
* Khôi phục / Xây dựng lại cấu trúc cây từ các kết quả duyệt
* Đường kính của cây nhị phân (Diameter of binary tree)
* Tổ tiên chung gần nhất (LCA - Lowest Common Ancestor)
* Đường đi có tổng lớn nhất trên cây (Maximum path sum)

### Cây tìm kiếm nhị phân (BST - Binary Search Tree)

* Đặc tính cốt lõi: cây con trái < nút gốc < cây con phải
* Thao tác tìm kiếm, chèn, xóa nút (tìm nút kế cận đứng sau successor / đứng trước predecessor)
* Xác thực tính hợp lệ của cây BST (Validate BST)
* Tìm phần tử nhỏ thứ $k$ / lớn thứ $k$ trên cây BST
* Truy vấn tổng trong một khoảng giá trị (Range sum queries)

### Cây tìm kiếm nhị phân cân bằng (Balanced BSTs - Lý thuyết)

* Cây AVL (Các phép quay cây, hệ số cân bằng balance factor)
* Cây đỏ đen (Red-Black trees - các quy tắc tô màu, ứng dụng thực tế)

### Đống nhị phân (Binary Heap)

* Biểu diễn cấu trúc đống dưới dạng mảng
* Thao tác trên đống cực tiểu (Min-heap) và đống cực đại (Max-heap)
* Thuật toán sắp xếp đống (Heap sort)
* Cài đặt cấu trúc Hàng đợi ưu tiên (Priority queue)

### Các cấu trúc cây nâng cao (Advanced Trees)

* Cây tiền tố Trie (Prefix tree) – các thao tác chèn, tìm kiếm, xóa, và cơ chế tự động gợi ý (auto-complete)
* Cây phân đoạn (Segment tree) – truy vấn khoảng (range queries), cập nhật điểm (point updates), lan truyền lười biếng (lazy propagation)
* Cây chỉ số nhị phân (Fenwick tree / Binary Indexed Tree)
* Cấu trúc tập hợp không giao nhau (Disjoint Set Union - Union-Find) – tối ưu hóa nén đường đi (path compression), hợp nhất theo hạng (union by rank)

---

## Chương 11: Đồ thị (Graphs)

### Cách biểu diễn đồ thị (Graph Representations)

* Ma trận kề (Adjacency matrix)
* Danh sách kề (Adjacency list)
* Danh sách cạnh (Edge list)

### Duyệt đồ thị (Graph Traversals)

* Duyệt theo chiều sâu (DFS - Depth First Search) – phiên bản đệ quy và lặp sử dụng ngăn xếp
* Duyệt theo chiều rộng (BFS - Breadth First Search) – sử dụng hàng đợi

### Các thuật toán tìm đường đi ngắn nhất (Shortest Path Algorithms)

* Đồ thị không trọng số: Duyệt theo chiều rộng BFS
* Đồ thị trọng số không âm: Thuật toán Dijkstra (sử dụng đống cực tiểu min-heap để tối ưu)
* Đồ thị có trọng số âm: Thuật toán Bellman-Ford
* Tìm đường đi ngắn nhất giữa mọi cặp đỉnh: Thuật toán Floyd-Warshall

### Cây khung tối tiểu (MST - Minimum Spanning Tree)

* Thuật toán Prim
* Thuật toán Kruskal (sử dụng Union-Find để tối ưu phát hiện chu trình)

### Sắp xếp topo (Topological Sorting)

* Thuật toán Kahn (dựa trên giải thuật BFS và bậc vào)
* Phương pháp sắp xếp topo dựa trên giải thuật DFS

### Phát hiện chu trình (Cycle Detection)

* Trên đồ thị vô hướng (sử dụng DFS hoặc cấu trúc dữ liệu Union-Find)
* Trên đồ thị có hướng (sử dụng DFS kết hợp ngăn xếp đệ quy hoặc thuật toán tô màu nút)

### Thành phần liên thông mạnh (SCC - Strongly Connected Components)

* Thuật toán Kosaraju
* Thuật toán Tarjan

### Luồng cực đại trên mạng (Network Flow - Cơ bản)

* Luồng cực đại / Lát cắt tối thiểu (Max flow / min cut, thuật toán Ford-Fulkerson và Edmonds-Karp)

### Các bài toán đồ thị kinh điển (Important Graph Problems)

* Sao chép đồ thị (Clone a graph)
* Bài toán biến đổi từ (Word ladder - sử dụng BFS)
* Lập lịch khóa học (Course schedule - sử dụng sắp xếp topo)
* Tìm số lượng hòn đảo (Number of islands - sử dụng DFS/BFS)
* Kiểm tra đồ thị hai phía (Bipartite graph checking)

---

## Chương 12: Quy hoạch động (Dynamic Programming - DP)

### Nguyên lý Quy hoạch động nền tảng

* Các bài toán con trùng nhau / chồng lấn (Overlapping subproblems)
* Cấu trúc con tối ưu (Optimal substructure)
* Kỹ thuật ghi nhớ từ trên xuống (Memoization - top-down) so với Lập bảng từ dưới lên (Tabulation - bottom-up)

### Các bài toán quy hoạch động kinh điển (Classic DP Problems)

* Dãy số Fibonacci
* Bài toán leo cầu thang (Climbing stairs)
* Bài toán trộm nhà (House robber - Quy hoạch động 1D)
* Tìm đường đi có tổng nhỏ nhất trên lưới tọa độ (Minimum path sum)
* Đếm số đường đi duy nhất trên lưới (Unique paths)

### Các bài toán về Chuỗi con và Xâu con (Subsequence and Substring)

* Chuỗi con chung dài nhất (LCS - Longest Common Subsequence)
* Xâu con chung dài nhất (Longest Common Substring)
* Dãy con tăng dài nhất (LIS - Longest Increasing Subsequence)
* Khoảng cách chỉnh sửa ký tự (Levenshtein / Edit distance)

### Bài toán Tập con và Cái túi (Subset and Knapsack)

* Bài toán Cái túi dạng 0/1 (0/1 Knapsack)
* Bài toán tổng tập con (Subset sum)
* Phân chia tập hợp thành hai phần có tổng bằng nhau (Partition equal subset sum)

### Quy hoạch động trên Chuỗi ký tự (DP on Strings)

* Phân tách chuỗi đối xứng (Palindrome partitioning)
* So khớp ký tự đại diện (Wildcard matching)
* So khớp biểu thức chính quy (Regular expression matching)

### Quy hoạch động trên cấu trúc Cây (DP on Trees)

* Tree DP (tìm tập độc lập lớn nhất - maximum independent set, tìm đường kính của cây)

### Quy hoạch động kết hợp trạng thái Bitmask (DP with Bitmask)

* Bài toán Người đi du lịch (TSP - Travelling Salesman Problem)

### Thuật toán Kadane (Dưới góc nhìn Quy hoạch động)

---

## Chương 13: Giải thuật tham lam (Greedy Algorithms)

* Đặc tính lựa chọn tham lam (Greedy choice property)
* Bài toán lựa chọn hoạt động (Activity selection)
* Bài toán cái túi dạng phân số (Fractional knapsack)
* Bài toán lập lịch công việc theo thời hạn tối đa (Job sequencing with deadlines)
* Mã hóa Huffman (Huffman coding - nén dữ liệu)
* Bài toán đổi tiền xu (So sánh hướng tiếp cận Tham lam vs Quy hoạch động)
* Tìm số lượng nhà ga tối thiểu (Lập lịch khoảng thời gian trống giao nhau - interval scheduling)

---

## Chương 14: Các chủ đề so sánh phỏng vấn cốt lõi (Important Interview Topics)

* So sánh mảng (Array) và Danh sách liên kết (Linked List)
* So sánh Ngăn xếp (Stack), Hàng đợi (Queue) và Đống (Heap)
* So sánh Đệ quy (Recursion) và Vòng lặp (Iteration)
* So sánh Duyệt đồ thị DFS và BFS
* So sánh Hash Map và Tree Map (Độ phức tạp, thứ tự lưu trữ)
* So sánh toàn diện các thuật toán sắp xếp (tính ổn định, sắp xếp tại chỗ, độ phức tạp thời gian/không gian)
* So sánh danh sách liên kết đơn (Singly) và kép (Doubly)
* So sánh Cây tìm kiếm nhị phân BST và Bảng băm (Hash Table)
* Khi nào nên áp dụng Quy hoạch động vs Tham lam vs Quay lui vs Chia để trị?
* Các ứng dụng thực tế quan trọng của cấu trúc dữ liệu Union-Find
* So sánh cấu trúc Trie và Bảng băm trong bài toán tìm kiếm chuỗi ký tự
* Phân biệt kỹ thuật Hai con trỏ (Two-pointer) và Cửa sổ trượt (Sliding Window)

---

## Chương 15: Phân tích độ phức tạp & Các dạng toán số học nâng cao (Complexity Analysis & Numerical Problems)

* Giải phương trình truy hồi và Định lý thợ (Master Theorem)
* Phân tích phân bổ / khấu hao (Amortized analysis - ví dụ: mảng động, bộ đếm tăng dần)
* Đánh đổi giữa không gian và thời gian (Space-time trade-offs)

### Các dạng toán số học (Numerical Problems)

* Tính lũy thừa nhanh của một số (Fast exponentiation)
* Sàng nguyên tố Eratosthenes (Sieve of Eratosthenes - sinh các số nguyên tố)
* Ước chung lớn nhất (GCD - thuật toán Euclid)
* Tính lũy thừa mô-đun nhanh (Modular exponentiation)
* Tính giai thừa của các số cực lớn (Giải pháp xử lý tràn số - handling overflow)
* Số Catalan (Catalan numbers - các bài toán đếm cấu trúc hợp lệ)

---

## Chương 16: Các chủ đề nâng cao và bổ trợ khác (Advanced & Miscellaneous Topics)

* Ngăn xếp đơn điệu (Monotonic stack - ứng dụng tìm phần tử lớn hơn tiếp theo, hình chữ nhật lớn nhất)
* Ứng dụng của hàng đợi hai đầu Deque (Tìm phần tử cực đại trong cửa sổ trượt)
* Cấu trúc đa tập hợp (Multiset) và tập hợp có thứ tự (Ordered set - Cây BST cân bằng tích hợp trong các thư viện chuẩn)
* Hàm băm cuốn (Rolling hash - Thuật toán so khớp Rabin-Karp)
* Thuật toán bầu cử đa số Boyer-Moore (Boyer-Moore majority vote algorithm)
* Thuật toán bỏ phiếu của Moore tìm phần tử chiếm đa số (Majority element)
* Lấy mẫu hồ chứa (Reservoir sampling - lấy mẫu ngẫu nhiên từ luồng dữ liệu liên tục)
* Gộp và phân chia khoảng thời gian giao nhau (Interval merging and partitioning)
* Bài toán đặt phòng họp (Meeting rooms II - tìm số lượng phòng họp tối thiểu cần thiết)
* Giải thuật ngẫu nhiên (Randomized algorithms - lựa chọn nhanh ngẫu nhiên quickselect, sắp xếp nhanh ngẫu nhiên randomized quicksort)

---

## Ủng hộ dự án (Support)

Nếu kho lưu trữ này mang lại giá trị hữu ích cho quá trình học tập của bạn, hãy dành tặng cho dự án một ngôi sao ⭐ để bày tỏ sự ủng hộ. Mọi đóng góp, sửa lỗi và bổ sung thêm danh sách các bài toán luyện tập luôn được chào đón nồng nhiệt.
