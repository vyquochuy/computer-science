# Chương 12: Quy hoạch động (Dynamic Programming - DP)

Quy hoạch động (Dynamic Programming - DP) là phương pháp giải các bài toán phức tạp bằng cách chia nhỏ chúng thành các bài toán con trùng lặp (overlapping subproblems), giải quyết từng bài toán con này đúng một lần và lưu trữ kết quả của chúng để tránh việc phải tính toán lại một cách dư thừa. Chương này sẽ trình bày từ các khái niệm cơ bản của Quy hoạch động, các lỗi thường gặp trong quá trình cài đặt, cách nhận diện các mẫu bài toán, các trường hợp KHÔNG nên sử dụng Quy hoạch động, cho đến các bài toán kinh điển như: bài toán dãy con và xâu con, bài toán cái túi (knapsack), Quy hoạch động trên xâu, Quy hoạch động trên cây, Quy hoạch động Bitmask (trạng thái), và thuật toán Kadane dưới góc nhìn Quy hoạch động.

## 1. Cơ bản về Quy hoạch động (DP Fundamentals)

### 1.1 Các khái niệm cốt lõi

- **Các bài toán con trùng lặp (Overlapping Subproblems)**: Bài toán lớn có thể được chia thành các bài toán con nhỏ hơn mà kết quả của chúng được tái sử dụng nhiều lần. Ví dụ điển hình là dãy số Fibonacci: công thức truy hồi $F(n) = F(n-1) + F(n-2)$ sẽ liên tục tính toán lại cùng một giá trị nhiều lần nếu cài đặt đệ quy thuần túy.
- **Cấu trúc con tối ưu (Optimal Substructure)**: Lời giải tối ưu của bài toán lớn có thể được xây dựng từ lời giải tối ưu của các bài toán con của nó. Ví dụ: Đường đi ngắn nhất từ $A$ đến $C$ đi qua $B$ – các đoạn đường đi từ $A \to B$ và $B \to C$ bắt buộc phải là các đường đi ngắn nhất.
- **Phương pháp ghi nhớ (Memoization - Top-Down)**: Tiếp cận theo hướng đệ quy có nhớ. Ta giải quyết bài toán bằng đệ quy và lưu trữ kết quả của mỗi bài toán con vào bộ nhớ đệm trước khi trả về giá trị, nhờ đó nếu gặp lại bài toán con đó, ta có thể lấy ngay kết quả trong thời gian $O(1)$.
- **Phương pháp lập bảng (Tabulation - Bottom-Up)**: Tiếp cận theo hướng lặp khử đệ quy. Ta xây dựng bảng phương án (DP table) bằng vòng lặp từ các bài toán con cơ sở nhỏ nhất đi dần lên bài toán lớn cần giải quyết.

### 1.2 Liên hệ thực tế

- **Phương pháp ghi nhớ (Memoization)**: Giống như việc bạn chép nháp các công thức hoặc kết quả trung gian trong phòng thi. Khi giải quyết xong một câu hỏi phụ, bạn ghi lại đáp án để nếu câu hỏi sau có phần liên quan, bạn chỉ việc đọc lại kết quả nháp thay vì phải ngồi giải lại từ đầu.
- **Phương pháp lập bảng (Tabulation)**: Giống như việc bạn điền vào bảng cửu chương theo từng dòng từ trên xuống dưới; mỗi ô mới được tính toán dựa trên các số liệu đã được điền sẵn ở các hàng trước đó.

### 1.3 Ví dụ minh họa qua dãy Fibonacci

**Phương pháp ghi nhớ (Memoization - Top-Down)**:

```cpp
vector<long long> memo(100, -1);
long long fibMemo(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    return memo[n] = fibMemo(n-1) + fibMemo(n-2);
}
```

**Phương pháp lập bảng (Tabulation - Bottom-Up)**:

```cpp
long long fibTab(int n) {
    if (n <= 1) return n;
    vector<long long> dp(n+1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; ++i)
        dp[i] = dp[i-1] + dp[i-2];
    return dp[n];
}
```

**Tối ưu hóa không gian lưu trữ (Space-Optimised)**:

```cpp
long long fibOpt(int n) {
    if (n <= 1) return n;
    int a = 0, b = 1, c;
    for (int i = 2; i <= n; ++i) {
        c = a + b;
        a = b;
        b = c;
    }
    return b;
}
```

## 2. Các lỗi thường gặp khi làm Quy hoạch động (Common DP Mistakes)

Ngay cả khi bạn đã tìm ra công thức truy hồi đúng, các giải pháp Quy hoạch động vẫn rất dễ bị sai sót trong quá trình cài đặt. Việc nhận diện được các lỗi này sẽ giúp ích rất nhiều trong quá trình gỡ lỗi (debugging) và phỏng vấn trực tiếp.

| Lỗi thường gặp | Mô tả lỗi | Ví dụ minh họa | Cách xử lý đúng |
|---------|-------------|---------|-------------------|
| **Khởi tạo sai giá trị cơ sở** | Các trường hợp cơ sở (base cases) hoặc các giá trị ban đầu trong bảng DP bị khởi tạo sai. | Trong bài toán LCS, khởi tạo `dp[0][j] = 1` thay vì khởi tạo bằng 0. | Khởi tạo đúng `dp[0][j] = 0` (vì tiền tố rỗng thì không có dãy con chung). |
| **Thứ tự duyệt vòng lặp không chính xác** | Trong phương pháp lập bảng, việc duyệt vòng lặp trong theo hướng không đúng cho bài toán cái túi 1 chiều (1D Knapsack). | Sử dụng vòng lặp xuôi `for (int w = 0; w <= W; ++w)` cho bài toán cái túi 0/1 – dẫn đến một vật phẩm bị chọn nhiều lần. | Duyệt ngược `w` từ `W` giảm dần về tới `weight[i]`. |
| **Lỗi lệch chỉ số một đơn vị (Off-by-one)** | Sử dụng `dp[i]` khi biến `i` đại diện cho độ dài hoặc chỉ số mảng một cách không chính xác. | Truy cập `dp[n]` trong khi kích thước thực tế của mảng `dp` chỉ được cấp phát là `n`. | Sử dụng `dp[n+1]` khi trạng thái đại diện cho độ dài của mảng. |
| **Bỏ sót việc xử lý các trạng thái vô nghiệm** | Sử dụng giá trị vô cùng `INT_MAX` trực tiếp mà không kiểm tra hiện tượng tràn số (overflow). | Thực hiện phép cộng `dp[i] = min(dp[i-1], dp[i-2]) + cost` mà không có cơ chế phòng ngừa khi giá trị trước đó là `INT_MAX`. | Sử dụng `INT_MAX/2` để tránh tràn số khi thực hiện phép cộng, hoặc kiểm tra điều kiện `dp[i-1] != INT_MAX`. |
| **Sử dụng đệ quy có nhớ mà quên xóa bộ nhớ đệm** | Sử dụng mảng ghi nhớ toàn cục (global memo array) cho nhiều bộ kiểm thử (test cases) khác nhau mà không reset lại mảng sau mỗi lượt chạy. | Mảng `memo` giữ nguyên các giá trị đã tính toán từ các lượt gọi hàm của bộ dữ liệu cũ. | Thực hiện reset lại mảng `memo` hoặc truyền mảng cục bộ vào mỗi lượt gọi hàm. |
| **Quên lưu trữ kết quả đã tính toán** | Hàm đệ quy thực hiện tính toán và trả về kết quả nhưng lại quên gán giá trị đó vào mảng ghi nhớ `memo` trước khi return. | Viết lệnh đệ quy trực tiếp `return fib(n-1) + fib(n-2)` mà không lưu vào `memo[n]`. | Thực hiện lưu trữ giá trị vào `memo[n]` trước khi trả về kết quả. |
| **Chọn sai số chiều của trạng thái Quy hoạch động** | Sử dụng quá ít số chiều dẫn đến không đủ thông tin để mô tả trạng thái (ví dụ bài toán cái túi có thêm ràng buộc về thể tích cần đến mảng 2 chiều). | Sử dụng mảng `dp[W]` cho bài toán cái túi có cả ràng buộc về trọng lượng và thể tích. | Cần sử dụng mảng 2 chiều `dp[W][V]`. |
| **Tối giản trạng thái quá sớm (Premature pruning)** | Bỏ qua một số trạng thái do giả định sai lầm rằng một trạng thái khác luôn tốt hơn. | Trong bài toán tổng tập con, giả định rằng việc đạt được tổng lớn hơn ở các bước trung gian luôn là phương án tốt hơn. | Cần đánh giá kỹ lưỡng tất cả các trạng thái có khả năng đạt tới. |

### Ví dụ minh họa: Lỗi lệch chỉ số một đơn vị (Off-by-One) trong bài toán leo cầu thang

**Cài đặt sai**:
```cpp
int climbStairs(int n) {
    vector<int> dp(n, 0);
    dp[0] = 1;
    dp[1] = 2; // Gây ra lỗi vượt quá chỉ số mảng (out of bounds) khi n == 1
    for (int i = 2; i < n; ++i) dp[i] = dp[i-1] + dp[i-2];
    return dp[n-1];
}
```

**Cài đặt đúng**:
```cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    vector<int> dp(n+1);
    dp[1] = 1; dp[2] = 2;
    for (int i = 3; i <= n; ++i) dp[i] = dp[i-1] + dp[i-2];
    return dp[n];
}
```

## 3. Nhận diện mẫu bài toán Quy hoạch động (DP Pattern Recognition)

Nhận biết được một bài toán có thể giải bằng Quy hoạch động hay không thường là bước khó khăn nhất. Dưới đây là các câu hỏi gợi ý và bảng tổng hợp giúp bạn nhanh chóng nhận diện dạng bài toán trong các buổi phỏng vấn hoặc thi lập trình thi đấu.

### 3.1 Các câu hỏi định hướng chính

- **Bài toán có thể phân rã thành các bài toán con nhỏ hơn không?** Việc tìm ra lời giải cho bài toán kích thước $n$ có phụ thuộc vào lời giải của các bài toán con có kích thước nhỏ hơn (ví dụ $n-1$, $n/2$) hay không?
- **Có tồn tại các bài toán con trùng lặp không?** Nếu áp dụng phương pháp đệ quy quay lui thông thường, chương trình có phải tính toán lại một giá trị đầu vào giống nhau nhiều lần hay không?
- **Bài toán có yêu cầu tối ưu hóa không?** (Tìm giá trị lớn nhất, nhỏ nhất, dài nhất, ngắn nhất, số cách thực hiện tối ưu, hoặc kiểm tra tính đúng/sai).
- **Bài toán có liên quan tới các đối tượng như dãy số, lưới (grid), hoặc tập hợp con không?** Đây là các miền bài toán Quy hoạch động cực kỳ phổ biến.
- **Bài toán có phải là một biến thể của các bài toán Quy hoạch động kinh điển không?** (Ví dụ LCS, cái túi, khoảng cách biến đổi, đổi tiền,...).

### 3.2 Bảng tra nhanh mẫu bài toán Quy hoạch động

| Đặc điểm bài toán | Dạng Quy hoạch động | Ví dụ định nghĩa trạng thái |
|-------------------------|--------------------|---------------------------|
| Một chuỗi các quyết định nối tiếp (chọn/bỏ qua, mua/bán) | Quy hoạch động 1 chiều (1D DP) | `dp[i]` = kết quả tốt nhất khi xét tới phần tử thứ $i$ |
| Di chuyển trên lưới ô vuông (chỉ đi sang phải/xuống dưới) | Quy hoạch động trên lưới 2 chiều | `dp[i][j]` = kết quả tối ưu tại ô $(i, j)$ |
| So sánh giữa hai chuỗi xâu ký tự | Quy hoạch động trên xâu (LCS, edit distance) | `dp[i][j]` = kết quả cho tiền tố $s_1[0..i-1]$ và $s_2[0..j-1]$ |
| Tổng tập con / Chọn đồ vật | Cái túi 0/1 hoặc cái túi vô hạn | `dp[w]` = giá trị lớn nhất đạt được với sức chứa $w$ |
| Đếm số cách cấu thành (không quan tâm thứ tự) | Quy hoạch động tổ hợp | `dp[x]` = số cách để tạo ra tổng lượng giá trị $x$ |
| Tìm điểm chia nhỏ nhất để phân hoạch | Quy hoạch động phân hoạch (phân hoạch đối xứng, nhân ma trận liên tiếp) | `dp[i]` = chi phí tối thiểu khi xét tiền tố đến vị trí $i$ |
| Cây có mối quan hệ phụ thuộc cha-con | Quy hoạch động trên cây | `dp[u][0/1]` = kết quả cho cây con gốc $u$ (chọn đỉnh $u$ hoặc không chọn) |
| Giới hạn kích thước bài toán rất nhỏ ($n \le 20$) | Quy hoạch động Bitmask (trạng thái) | `dp[mask][i]` = kết quả tốt nhất khi đã duyệt tập đỉnh tương ứng với `mask` và kết thúc tại đỉnh $i$ |
| Tính xác suất / Giá trị kỳ vọng | Quy hoạch động xác suất | `dp[i]` = giá trị kỳ vọng đạt được từ trạng thái $i$ |

### 3.3 Ví dụ minh họa quy trình nhận diện mẫu bài toán

**Bài toán**: Cho một mảng số nguyên, hãy tìm độ dài của dãy con tăng dài nhất.

- **Câu hỏi**: Có thể tìm dãy con tăng dài nhất kết thúc tại vị trí $i$ dựa trên các tiền tố ngắn hơn không? Có: LIS kết thúc tại $i$ phụ thuộc vào tất cả các vị trí $j < i$ thỏa mãn điều kiện $nums[j] < nums[i]$.
- **Có các bài toán con trùng lặp không?** Có, vì với các chỉ số $i$ khác nhau, chúng ta đều phải lặp đi lặp lại việc kiểm tra các chỉ số $j$ trước đó.
- **Có yêu cầu tối ưu hóa không?** Có, yêu cầu tìm độ dài "dài nhất".
- **Dạng bài toán quen thuộc?** LIS – Dãy con tăng dài nhất là một bài toán Quy hoạch động kinh điển với độ phức tạp $O(n^2)$ hoặc $O(n \log n)$ sử dụng kỹ thuật xếp bài (patience sorting).

**Kết luận**: Sử dụng Quy hoạch động.

### 3.4 Quy trình chuyển đổi từ giải pháp Duyệt thô sang Quy hoạch động

1. Thiết kế giải pháp đệ quy quay lui (backtracking) vét cạn để duyệt qua tất cả các trường hợp có thể xảy ra.
2. Xác định các tham số (parameters) thay đổi trong quá trình gọi đệ quy.
3. Sử dụng các tham số thay đổi đó làm các chiều trạng thái trong bảng Quy hoạch động.
4. Áp dụng phương pháp ghi nhớ (Memoization) để lưu trữ kết quả của các tham số này.
5. (Tùy chọn) Chuyển đổi mã nguồn từ đệ quy có nhớ sang dạng lập bảng (tabulation) lặp khử đệ quy để tối ưu hiệu năng.

## 4. Khi nào KHÔNG nên sử dụng Quy hoạch động (When NOT to Use DP)

Quy hoạch động rất mạnh mẽ, tuy nhiên nó không phải lúc nào cũng là giải pháp tối ưu hoặc cần thiết nhất. Việc nhận biết khi nào nên dùng các kỹ thuật khác đơn giản và hiệu quả hơn sẽ giúp bạn tiết kiệm thời gian và giảm thiểu độ phức tạp của mã nguồn.

### 4.1 Thuật toán Tham lam (Greedy) cho hiệu quả tốt hơn

Nếu bài toán có **thuộc tính lựa chọn tham lam** (greedy choice property - việc chọn lựa phương án tối ưu cục bộ tại mỗi bước luôn dẫn đến giải pháp tối ưu toàn cục), thì giải pháp tham lam thường sẽ đơn giản và nhanh hơn rất nhiều (độ phức tạp $O(n)$ hoặc $O(n \log n)$ so với Quy hoạch động thường là $O(n^2)$ hoặc cao hơn).

**Các ví dụ điển hình**:
- **Bài toán đổi tiền** (với các hệ thống tiền tệ chuẩn như tiền tệ của Mỹ hay Việt Nam): Thuật toán tham lam luôn cho kết quả đúng. Quy hoạch động chỉ thực sự cần thiết khi hệ mệnh giá tiền là bất kỳ.
- **Lựa chọn hoạt động** (chọn số lượng khoảng thời gian không chồng chéo nhau nhiều nhất): Sử dụng tham lam ưu tiên thời điểm kết thúc sớm nhất.
- **Số bước nhảy tối thiểu để về đích** (khi biết độ dài bước nhảy tối đa tại mỗi vị trí): Thuật toán tham lam giải quyết được trong $O(n)$.
- **Bài toán cái túi dạng phân số (Fractional Knapsack)**: Dùng tham lam sắp xếp theo tỷ lệ giá trị/trọng lượng. Còn bài toán cái túi dạng số nguyên (0/1 Knapsack) bắt buộc phải dùng Quy hoạch động.

**Cách nhận biết**: Thuật toán tham lam hoạt động tốt khi việc chọn một phần tử không áp đặt ràng buộc quá phức tạp lên các lựa chọn tiếp theo đến mức buộc chúng ta phải duyệt qua toàn bộ các tập con khả dĩ. Thông thường yêu cầu đề bài sẽ là "chọn nhiều nhất có thể" hoặc "tối đa hóa" mà không có sự ràng buộc tổng lượng giá trị tích lũy một cách chặt chẽ.

### 4.2 Kỹ thuật Cửa sổ trượt (Sliding Window) là đủ

Nếu bài toán yêu cầu tìm kiếm phương án tối ưu trên các **dãy con liên tiếp (contiguous subarray hoặc substring)** và điều kiện ràng buộc mang tính đơn điệu (monotonic) (ví dụ: chứa tối đa $K$ ký tự khác nhau, tổng giá trị $\le K$), thì kỹ thuật cửa sổ trượt sẽ cho độ phức tạp thời gian là $O(n)$ và không gian lưu trữ là $O(1)$ – tốt hơn rất nhiều so với Quy hoạch động.

**Các ví dụ điển hình**:
- **Xâu con dài nhất có tối đa $K$ ký tự phân biệt**: Sử dụng cửa sổ trượt.
- **Dãy con liên tiếp ngắn nhất có tổng lớn hơn hoặc bằng $K$**: Sử dụng cửa sổ trượt.
- **Dãy con liên tiếp có tổng lớn nhất với kích thước cố định $k$**: Sử dụng cửa sổ trượt (hoặc mảng cộng dồn prefix sum).
- **Thu thập trái cây vào giỏ** (tối đa 2 loại quả): Sử dụng cửa sổ trượt.

**Cách nhận biết**: Đề bài có chứa các từ khóa "liên tiếp" (contiguous), "mảng con" (subarray), "xâu con" (substring) đi kèm ràng buộc có thể duy trì được bằng cách mở rộng hoặc thu hẹp hai đầu của một cửa sổ. Sử dụng Quy hoạch động sẽ làm bài toán trở nên quá phức tạp không cần thiết (ví dụ tốn $O(n^2)$ vô ích).

### 4.3 Tối ưu hóa bằng Tìm kiếm nhị phân

Đối với các bài toán yêu cầu tìm giá trị **nhỏ nhất của các giá trị lớn nhất (minimum of maximum)** hoặc **lớn nhất của các giá trị nhỏ nhất (maximum of minimum)**, phương pháp tìm kiếm nhị phân trên tập kết quả (binary search on the answer) kết hợp kiểm tra tính khả thi (thường tốn $O(n)$ hoặc $O(n \log n)$) sẽ đơn giản hơn Quy hoạch động rất nhiều.

**Các ví dụ điển hình**:
- **Chia mảng có tổng phân đoạn lớn nhất là nhỏ nhất**: Chia mảng thành $m$ phân đoạn liên tiếp sao cho tổng lớn nhất của một phân đoạn là nhỏ nhất. Giải bằng tìm kiếm nhị phân kết hợp kiểm tra tham lam.
- **Koko ăn chuối**: Tìm tốc độ ăn chuối tối thiểu để hoàn thành trong vòng $H$ giờ. Giải bằng tìm kiếm nhị phân kết hợp mô phỏng.
- **Khả năng vận chuyển hàng hóa trong vòng $D$ ngày**: Tìm kiếm nhị phân kết hợp tham lam.
- **Trung vị của hai mảng đã sắp xếp**: Tìm kiếm nhị phân trên mảng có kích thước nhỏ hơn.

**Cách nhận biết**: Kết quả cần tìm là một giá trị số đơn nhất và bạn có thể dễ dàng kiểm tra xem một giá trị ứng viên cụ thể có khả thi hay không trong thời gian $O(n)$. Hàm kiểm tra tính khả thi `isFeasible(x)` mang tính đơn điệu (sai với các giá trị $x$ nhỏ và đúng với các giá trị $x$ lớn hơn). Quy hoạch động trong trường hợp này sẽ cực kỳ phức tạp và chạy rất chậm.

### 4.4 Tồn tại Công thức Toán học hoặc Tổ hợp trực tiếp

Một số bài toán có công thức toán học tường minh (closed‑form solution) dạng $O(1)$ hoặc công thức tổ hợp đơn giản, giúp giải quyết trực tiếp mà không cần đến Quy hoạch động.

**Các ví dụ điển hình**:
- **Số cách bước lên đỉnh cầu thang có $n$ bậc** (mỗi lần bước 1 hoặc 2 bậc) $\to$ Thực chất là số Fibonacci thứ $n$ (có thể giải bằng Quy hoạch động hoặc sử dụng công thức Binet).
- **Số đường đi duy nhất trên lưới ô vuông** – Có công thức tổ hợp tường minh bằng cách dùng giai thừa: $C(m+n-2, m-1)$.
- **Số Catalan thứ $N$** – Có công thức toán học trực tiếp, mặc dù Quy hoạch động vẫn thường được dùng để minh họa học thuật.

**Cách nhận biết**: Bài toán yêu cầu tính toán thuần túy số cách hoặc một giá trị cụ thể trùng khớp với một chuỗi số toán học đã biết.

### 4.5 So sánh hiệu năng tổng quan

Quy hoạch động vẫn có thể cho ra kết quả **đúng** trong tất cả các trường hợp trên, nhưng nó sẽ kéo theo độ phức tạp thời gian/không gian lớn hơn và viết nhiều mã nguồn hơn. Bảng dưới đây tóm tắt các lựa chọn thay thế:

| Kỹ thuật thay thế | Trường hợp áp dụng | Độ phức tạp điển hình | Độ phức tạp nếu dùng Quy hoạch động |
|-----------|-------------|--------------------|---------------------------|
| **Greedy** | Bài toán có thuộc tính lựa chọn tham lam | $O(n)$ hoặc $O(n \log n)$ | $O(n^2)$ hoặc cao hơn |
| **Sliding Window** | Dãy con liên tiếp với điều kiện đơn điệu | $O(n)$ | $O(n^2)$ (ví dụ: tìm xâu con đối xứng dài nhất) |
| **Tìm kiếm nhị phân kết quả** | Bài toán tìm cực trị (min-max/max-min) có tính đơn điệu | $O(n \log M)$ | Thường là $O(n^2)$ hoặc lũy thừa |
| **Công thức Toán học** | Tồn tại lời giải toán học dạng tường minh trực tiếp | $O(1)$ | $O(n)$ hoặc $O(n^2)$ |

**Quy tắc chung**: Nếu bạn nghi ngờ bài toán có thể giải bằng tham lam hoặc cửa sổ trượt, hãy thử kiểm chứng nhanh với một vài ví dụ nhỏ. Nếu tính đúng đắn được giữ vững, hãy bỏ qua Quy hoạch động. Ngược lại, nếu đề bài yêu cầu liệt kê hoặc tối ưu hóa trên "tất cả các trường hợp có thể kết hợp" thì Quy hoạch động (hoặc quay lui) là lựa chọn bắt buộc.

## 5. Các bài toán Quy hoạch động kinh điển (Classic DP Problems)

### 5.1 Leo cầu thang (Climbing Stairs)

**Bài toán**: Đếm số cách để leo lên đỉnh của một cầu thang có $n$ bậc, biết mỗi lần bạn chỉ có thể bước lên 1 bậc hoặc 2 bậc.

**Hệ thức truy hồi**: `dp[i] = dp[i-1] + dp[i-2]` với các trường hợp cơ sở `dp[0] = 1, dp[1] = 1`.

```cpp
int climbStairs(int n) {
    if (n <= 1) return 1;
    int a = 1, b = 1, c;
    for (int i = 2; i <= n; ++i) {
        c = a + b;
        a = b;
        b = c;
    }
    return b;
}
```

### 5.2 Trộm nhà (House Robber - 1D DP)

**Bài toán**: Tìm cách trộm vàng từ các ngôi nhà nằm trên một con đường sao cho tổng lượng vàng thu được là lớn nhất, biết rằng không được phép trộm ở hai ngôi nhà nằm kề nhau.

**Hệ thức truy hồi**: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

```cpp
int rob(vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return 0;
    if (n == 1) return nums[0];
    int prev2 = nums[0], prev1 = max(nums[0], nums[1]);
    for (int i = 2; i < n; ++i) {
        int curr = max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### 5.3 Đường đi có tổng nhỏ nhất (Minimum Path Sum - Trên lưới)

**Bài toán**: Tìm một đường đi từ ô góc trên bên trái đến ô góc dưới bên phải trên một lưới số nguyên sao cho tổng các số trên đường đi là nhỏ nhất (chỉ cho phép di chuyển sang phải hoặc xuống dưới).

**Hệ thức truy hồi**: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n));
    dp[0][0] = grid[0][0];
    for (int i = 1; i < m; ++i) dp[i][0] = dp[i-1][0] + grid[i][0];
    for (int j = 1; j < n; ++j) dp[0][j] = dp[0][j-1] + grid[0][j];
    for (int i = 1; i < m; ++i)
        for (int j = 1; j < n; ++j)
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]);
    return dp[m-1][n-1];
}
```

### 5.4 Đường đi duy nhất (Unique Paths - Trên lưới)

**Bài toán**: Đếm số lượng đường đi phân biệt từ ô góc trên bên trái đến ô góc dưới bên phải trên một lưới kích thước $m \times n$ (chỉ cho phép di chuyển sang phải hoặc xuống dưới).

**Hệ thức truy hồi**: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.

```cpp
int uniquePaths(int m, int n) {
    vector<int> dp(n, 1);
    for (int i = 1; i < m; ++i)
        for (int j = 1; j < n; ++j)
            dp[j] += dp[j-1];
    return dp[n-1];
}
```

## 6. Các bài toán về Dãy con và Xâu con (Subsequence and Substring Problems)

### 6.1 Dãy con chung dài nhất (LCS - Longest Common Subsequence)

**Bài toán**: Tìm độ dài lớn nhất của một dãy con chung giữa hai xâu ký tự (dãy con không nhất thiết phải nằm liên tiếp nhau).

**Hệ thức truy hồi**:
$$\text{dp}[i][j] = \begin{cases} \text{dp}[i-1][j-1] + 1 & \text{nếu } s_1[i-1] == s_2[j-1] \\ \max(\text{dp}[i-1][j], \text{dp}[i][j-1]) & \text{ngược lại} \end{cases}$$

```cpp
int longestCommonSubsequence(string s1, string s2) {
    int m = s1.size(), n = s2.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            if (s1[i-1] == s2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

### 6.2 Xâu con chung dài nhất (Longest Common Substring)

**Hệ thức truy hồi**: `dp[i][j] = dp[i-1][j-1] + 1` nếu ký tự trùng khớp, ngược lại bằng 0. Giá trị cần tìm là giá trị lớn nhất xuất hiện trong toàn bộ bảng `dp`.

### 6.3 Dãy con tăng dài nhất (LIS - Longest Increasing Subsequence)

**Bài toán**: Tìm độ dài của dãy con tăng dài nhất trong một mảng số nguyên cho trước.

**Cài đặt bằng Quy hoạch động thông thường độ phức tạp $O(n^2)$**:

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);
    int best = 1;
    for (int i = 1; i < n; ++i) {
        for (int j = 0; j < i; ++j)
            if (nums[j] < nums[i])
                dp[i] = max(dp[i], dp[j] + 1);
        best = max(best, dp[i]);
    }
    return best;
}
```

**Cài đặt tối ưu độ phức tạp $O(n \log n)$ sử dụng Tìm kiếm nhị phân (Patience Sorting)**:

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}
```

### 6.4 Khoảng cách biến đổi (Edit Distance / Levenshtein Distance)

**Bài toán**: Tìm số bước tối thiểu (bao gồm các phép toán: thêm ký tự, xóa ký tự, thay thế ký tự) để biến đổi xâu ký tự $A$ thành xâu ký tự $B$.

**Hệ thức truy hồi**:
$$\text{dp}[i][j] = \begin{cases} \text{dp}[i-1][j-1] & \text{nếu } A[i-1] == B[j-1] \\ 1 + \min(\text{dp}[i-1][j], \text{dp}[i][j-1], \text{dp}[i-1][j-1]) & \text{ngược lại} \end{cases}$$

```cpp
int minDistance(string word1, string word2) {
    int m = word1.size(), n = word2.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i = 0; i <= m; ++i) dp[i][0] = i;
    for (int j = 0; j <= n; ++j) dp[0][j] = j;
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            if (word1[i-1] == word2[j-1])
                dp[i][j] = dp[i-1][j-1];
            else
                dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
    return dp[m][n];
}
```

## 7. Các bài toán Tổng tập con và Cái túi (Subset and Knapsack Problems)

### 7.1 Bài toán Cái túi 0/1 (0/1 Knapsack)

**Bài toán**: Lựa chọn các đồ vật bỏ vào túi có sức chứa tối đa là $W$ sao cho tổng giá trị thu được là lớn nhất, biết mỗi đồ vật chỉ được chọn tối đa một lần (0 hoặc 1).

**Hệ thức truy hồi**: `dp[w]` = giá trị tối đa đạt được với sức chứa chính xác là `w` (sử dụng mảng 1 chiều tối ưu không gian lưu trữ).

```cpp
int knapsack01(vector<int>& weights, vector<int>& values, int W) {
    int n = weights.size();
    vector<int> dp(W+1, 0);
    for (int i = 0; i < n; ++i)
        for (int w = W; w >= weights[i]; --w)
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i]);
    return dp[W];
}
```

### 7.2 Tổng tập con (Subset Sum)

**Bài toán**: Xác định xem có tồn tại một tập hợp con trong mảng số nguyên có tổng các phần tử bằng đúng giá trị mục tiêu (target) cho trước hay không.

```cpp
bool subsetSum(vector<int>& nums, int target) {
    vector<bool> dp(target+1, false);
    dp[0] = true;
    for (int x : nums)
        for (int s = target; s >= x; --s)
            if (dp[s - x]) dp[s] = true;
    return dp[target];
}
```

### 7.3 Chia mảng thành hai phần có tổng bằng nhau (Partition Equal Subset Sum)

**Bài toán**: Kiểm tra xem một mảng số nguyên có thể phân chia thành hai tập con sao cho tổng các phần tử của hai tập con đó bằng nhau hay không.

**Hướng tiếp cận**: Quy về bài toán tìm tập con có tổng bằng chính xác một nửa tổng giá trị của toàn bộ mảng (`total_sum / 2`).

```cpp
bool canPartition(vector<int>& nums) {
    int sum = accumulate(nums.begin(), nums.end(), 0);
    if (sum % 2) return false;
    return subsetSum(nums, sum/2);
}
```

## 8. Quy hoạch động trên Xâu ký tự (DP on Strings)

### 8.1 Phân hoạch xâu đối xứng tối thiểu (Palindrome Partitioning - Minimum Cuts)

**Bài toán**: Tìm số lần cắt ít nhất để phân chia một xâu ký tự thành các xâu con đối xứng (palindromes).

**Hướng tiếp cận**: Đầu tiên, tiền xử lý và lập bảng xác định xem mọi xâu con `s[i..j]` có phải là xâu đối xứng hay không. Sau đó áp dụng công thức: `dp[i]` = số lát cắt tối thiểu cho tiền tố xâu `s[0..i]`.

```cpp
int minCut(string s) {
    int n = s.size();
    vector<vector<bool>> isPal(n, vector<bool>(n, false));
    for (int len = 1; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            if (s[i] == s[j] && (len <= 2 || isPal[i+1][j-1]))
                isPal[i][j] = true;
        }
    vector<int> dp(n, INT_MAX);
    for (int i = 0; i < n; ++i) {
        if (isPal[0][i]) dp[i] = 0;
        else {
            for (int j = 0; j < i; ++j)
                if (isPal[j+1][i])
                    dp[i] = min(dp[i], dp[j] + 1);
        }
    }
    return dp[n-1];
}
```

### 8.2 Khớp ký tự đại diện (Wildcard Matching)

**Bài toán**: Kiểm tra xem một xâu ký tự đầu vào có khớp với mẫu ký tự đại diện chứa dấu `?` (khớp với chính xác 1 ký tự bất kỳ) và dấu `*` (khớp với một chuỗi ký tự bất kỳ có độ dài $\ge 0$) hay không.

**Hệ thức truy hồi**: `dp[i][j]` = đúng nếu đoạn tiền tố xâu `s[0..i-1]` khớp hoàn toàn với mẫu `p[0..j-1]`.

```cpp
bool isMatch(string s, string p) {
    int m = s.size(), n = p.size();
    vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
    dp[0][0] = true;
    for (int j = 1; j <= n; ++j)
        if (p[j-1] == '*') dp[0][j] = dp[0][j-1];
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            if (p[j-1] == '*' && (dp[i-1][j] || dp[i][j-1])) dp[i][j] = true;
            else if (p[j-1] == '?' || s[i-1] == p[j-1])
                dp[i][j] = dp[i-1][j-1];
    return dp[m][n];
}
```

### 8.3 Khớp biểu thức chính quy (Regular Expression Matching)

Tương tự như khớp ký tự đại diện, nhưng dấu `*` ở đây mang ý nghĩa lặp lại ký tự đứng ngay trước nó từ 0 hoặc nhiều lần.

## 9. Quy hoạch động trên Cây (DP on Trees)

### Tập độc lập lớn nhất trên Cây (Maximum Independent Set in Tree)

**Bài toán**: Chọn ra một tập các đỉnh trên cây sao cho không có hai đỉnh nào được chọn kề cạnh nhau và tổng giá trị của các đỉnh được chọn là lớn nhất.

**Hướng tiếp cận**: Với mỗi đỉnh $u$, ta tính hai trạng thái: `dp[u][1]` là tổng giá trị tối đa thu được khi CHỌN đỉnh $u$, và `dp[u][0]` là tổng tối đa thu được khi KHÔNG CHỌN đỉnh $u$.

**Truy hồi trạng thái**:
- `dp[u][0]` = tổng giá trị lớn nhất thu được từ các đỉnh con $v$: $\sum \max(\text{dp}[v][0], \text{dp}[v][1])$
- `dp[u][1]` = giá trị của chính đỉnh $u$ + tổng giá trị thu được khi không chọn các đỉnh con $v$: $\text{val}[u] + \sum \text{dp}[v][0]$

```cpp
void dfs(int u, int parent, vector<vector<int>>& adj, vector<int>& val, vector<vector<int>>& dp) {
    dp[u][1] = val[u];
    dp[u][0] = 0;
    for (int v : adj[u]) {
        if (v == parent) continue;
        dfs(v, u, adj, val, dp);
        dp[u][0] += max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
}
// Kết quả cuối cùng = max(dp[root][0], dp[root][1])
```

## 10. Quy hoạch động Bitmask – Bài toán người du lịch (TSP)

**Bài toán**: Tìm đường đi ngắn nhất đi qua tất cả các thành phố đúng một lần rồi quay trở lại điểm xuất phát ban đầu.

**Trạng thái Quy hoạch động**: `dp[mask][i]` = khoảng cách tối thiểu để đã đi qua tập các thành phố biểu diễn bởi `mask` (dưới dạng bit nhị phân) và kết thúc chặng đi tại thành phố $i$.

```cpp
int tsp(vector<vector<int>>& dist) {
    int n = dist.size();
    int FULL = (1 << n) - 1;
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX/2));
    dp[1][0] = 0; // Bắt đầu hành trình từ thành phố 0
    for (int mask = 1; mask < (1 << n); ++mask) {
        for (int i = 0; i < n; ++i) {
            if (!(mask & (1 << i)) || dp[mask][i] >= INT_MAX/2) continue;
            for (int j = 0; j < n; ++j) {
                if (mask & (1 << j)) continue;
                dp[mask | (1 << j)][j] = min(dp[mask | (1 << j)][j], dp[mask][i] + dist[i][j]);
            }
        }
    }
    int ans = INT_MAX;
    for (int i = 0; i < n; ++i)
        ans = min(ans, dp[FULL][i] + dist[i][0]);
    return ans;
}
```

- **Độ phức tạp thời gian**: $O(2^n \cdot n^2)$. **Không gian lưu trữ**: $O(2^n \cdot n)$.  
- **Liên hệ thực tế**: Lập lộ trình tối ưu cho shipper giao hàng tận nhà – sắp xếp thứ tự giao hàng cho các khách hàng sao cho tổng quãng đường di chuyển là ngắn nhất.

## 11. Thuật toán Kadane dưới góc nhìn Quy hoạch động

Thuật toán Kadane là một thuật toán Quy hoạch động 1 chiều cực kỳ nổi tiếng để tìm dãy con liên tiếp có tổng lớn nhất.

**Hệ thức truy hồi**: `dp[i] = max(nums[i], dp[i-1] + nums[i])` trong đó `dp[i]` đại diện cho tổng lớn nhất của dãy con liên tiếp kết thúc tại chỉ số $i$.

```cpp
int maxSubarraySum(vector<int>& nums) {
    int maxEndingHere = nums[0], maxSoFar = nums[0];
    for (int i = 1; i < nums.size(); ++i) {
        maxEndingHere = max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

## 12. Bảng tổng hợp các dạng bài toán

| Danh mục bài toán | Bài toán điển hình | Công thức truy hồi / Hướng tiếp cận | Độ phức tạp thời gian |
|------------------|----------------|------------------------|------------------|
| Quy hoạch động 1 chiều | Leo cầu thang | `dp[i] = dp[i-1] + dp[i-2]` | $O(n)$ |
| Lưới 2 chiều | Đường đi tổng nhỏ nhất | `dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]` | $O(mn)$ |
| Dãy con | Dãy con chung dài nhất (LCS) | Khớp ký tự $\to$ cộng 1; Không khớp $\to$ lấy $\max$ | $O(mn)$ |
| Dãy con | Dãy con tăng dài nhất (LIS) | `dp[i] = max(dp[j]) + 1` với mọi $j < i$ | $O(n^2)$ hoặc $O(n \log n)$ |
| Khoảng cách chuỗi | Khoảng cách Levenshtein | `1 + min(thêm, xóa, thay thế)` | $O(mn)$ |
| Chọn đồ vật | Cái túi 0/1 | `dp[w] = max(dp[w], dp[w-wt] + val)` | $O(nW)$ |
| Xâu ký tự | Khớp ký tự đại diện | `dp[i][j]` dựa trên cơ chế xử lý dấu `*` | $O(mn)$ |
| Quy hoạch động trên cây | Tập độc lập lớn nhất | Xét hai trạng thái: đỉnh con được chọn / không được chọn | $O(V + E)$ |
| Bitmask DP | Bài toán người du lịch (TSP) | `dp[mask][i] = min` qua các nút $j$ trung gian | $O(2^n \cdot n^2)$ |
| Quy hoạch động 1 chiều | Thuật toán Kadane | `maxEndingHere = max(x, cur + x)` | $O(n)$ |

Chương tiếp theo sẽ trình bày về các kỹ thuật thuật toán nâng cao cực kỳ quan trọng khác (thuật toán tham lam greedy, chia để trị divide and conquer, kỹ thuật hai con trỏ two pointers, và cấu trúc Union-Find DSU) cùng các ứng dụng thực tế của chúng.
