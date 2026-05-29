# Chương 16: Các chủ đề Nâng cao & Tổng hợp (Advanced & Miscellaneous Topics)

Chương này trình bày các thuật toán và cấu trúc dữ liệu nâng cao thường xuyên xuất hiện trong các buổi phỏng vấn kỹ thuật cấp trung đến nâng cao, nhưng không nằm riêng biệt ở các chương chuyên đề trước. Nội dung bao gồm: ngăn xếp đơn điệu (monotonic stack), hàng đợi hai đầu (deque) cho bài toán cửa sổ trượt, tập hợp có thứ tự (ordered set), kỹ thuật băm cuộn (rolling hash), thuật toán bầu chọn đa số (majority vote), lấy mẫu hồ chứa (reservoir sampling), hợp nhất các khoảng thời gian (interval merging), lập lịch phòng họp (meeting rooms II) và các thuật toán ngẫu nhiên hóa (randomized algorithms).

## 1. Ngăn xếp đơn điệu (Monotonic Stack)

**Khái niệm**: Là một ngăn xếp (Stack) duy trì các phần tử luôn ở trạng thái tăng dần hoặc giảm dần. Nó được sử dụng để tìm phần tử lớn hơn (hoặc nhỏ hơn) tiếp theo của mỗi vị trí trong mảng với độ phức tạp thời gian cực kỳ tối ưu là $O(n)$.

### 1.1 Phần tử lớn hơn tiếp theo (Next Greater Element)

**Bài toán**: Với mỗi phần tử trong mảng số nguyên, hãy tìm phần tử đầu tiên nằm ở phía bên phải có giá trị lớn hơn phần tử đó.

**Thuật toán**: Duyệt mảng từ trái qua phải, đồng thời duy trì một ngăn xếp lưu trữ các chỉ số (indices) sao cho giá trị tương ứng giảm dần (từ đáy đến đỉnh). Với mỗi phần tử mới đang xét, ta liên tục loại bỏ (pop) các chỉ số khỏi ngăn xếp nếu giá trị tại đỉnh ngăn xếp nhỏ hơn phần tử mới – khi đó, phần tử mới chính là phần tử lớn hơn tiếp theo của các chỉ số bị loại bỏ này.

```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st; // Lưu trữ chỉ số mảng
    for (int i = 0; i < n; ++i) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}
```

- **Độ phức tạp thời gian**: $O(n)$. **Độ phức tạp bộ nhớ**: $O(n)$.
- **Liên hệ thực tế**: Trong một hàng người xếp hàng, tìm người đứng phía trước đầu tiên có chiều cao cao hơn mình.

### 1.2 Hình chữ nhật lớn nhất trong biểu đồ cột (Largest Rectangle in Histogram)

**Bài toán**: Cho mảng biểu diễn chiều cao của các cột nằm sát kề nhau, hãy tìm diện tích hình chữ nhật lớn nhất có thể vẽ được bên trong biểu đồ cột này.

**Hướng tiếp cận**: Sử dụng một ngăn xếp đơn điệu tăng dần (lưu trữ chỉ số có chiều cao tăng dần). Khi gặp một cột có chiều cao nhỏ hơn cột ở đỉnh ngăn xếp, ta lần lượt pop các cột cao hơn ra và tính diện tích hình chữ nhật tương ứng với chiều cao của cột vừa bị pop làm giới hạn chiều cao nhỏ nhất.

```cpp
int largestRectangleArea(vector<int>& heights) {
    stack<int> st;
    int maxArea = 0;
    heights.push_back(0); // Cột lính canh (sentinel) để xả sạch ngăn xếp cuối cùng
    for (int i = 0; i < heights.size(); ++i) {
        while (!st.empty() && heights[st.top()] > heights[i]) {
            int h = heights[st.top()]; st.pop();
            int left = st.empty() ? -1 : st.top();
            maxArea = max(maxArea, h * (i - left - 1));
        }
        st.push(i);
    }
    heights.pop_back();
    return maxArea;
}
```

- **Độ phức tạp thời gian**: $O(n)$. **Độ phức tạp bộ nhớ**: $O(n)$.

## 2. Ứng dụng Hàng đợi hai đầu – Giá trị lớn nhất trên cửa sổ trượt (Deque – Sliding Window Maximum)

**Bài toán**: Tìm giá trị lớn nhất trong mọi mảng con liên tiếp (cửa sổ trượt) có kích thước bằng $k$ của một mảng số nguyên.

**Hướng tiếp cận**: Sử dụng một hàng đợi hai đầu (Deque) để lưu trữ chỉ số của các phần tử có khả năng làm giá trị lớn nhất. Các phần tử trong Deque luôn được duy trì sao cho giá trị tương ứng giảm dần. Với mỗi chỉ số mới $i$:
- Loại bỏ khỏi đầu (front) Deque các chỉ số đã trượt ra ngoài phạm vi cửa sổ hiện tại (nhỏ hơn hoặc bằng $i - k$).
- Loại bỏ khỏi cuối (back) Deque các chỉ số có giá trị nhỏ hơn hoặc bằng phần tử mới `nums[i]` (vì chúng chắc chắn không bao giờ có cơ hội làm giá trị lớn nhất nữa).
- Đẩy chỉ số $i$ hiện tại vào cuối Deque.
- Phần tử ở đầu Deque chính là giá trị lớn nhất của cửa sổ hiện tại (kể từ khi $i \ge k - 1$).

```cpp
vector<int> slidingWindowMaximum(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> result;
    for (int i = 0; i < nums.size(); ++i) {
        if (!dq.empty() && dq.front() == i - k) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) result.push_back(nums[dq.front()]);
    }
    return result;
}
```

- **Độ phức tạp thời gian**: $O(n)$. **Độ phức tạp bộ nhớ**: $O(k)$.
- **Liên hệ thực tế**: Một chiếc kính lúp trượt trên một băng số – bạn luôn theo dõi và giữ số lớn nhất đang xuất hiện bên trong kính lúp ở đầu hàng đợi.

## 3. Cấu trúc Đa tập hợp và Tập hợp có thứ tự (Cây tìm kiếm nhị phân cân bằng)

**Khái niệm**: Các cấu trúc dữ liệu duy trì tập hợp các phần tử luôn ở trạng thái đã sắp xếp và hỗ trợ các thao tác thêm, xóa, tìm kiếm trong thời gian tối ưu $O(\log n)$. Trong ngôn ngữ C++, ta sử dụng `std::set` (lưu khóa duy nhất) và `std::multiset` (cho phép các khóa trùng nhau), được cài đặt bên dưới bằng cấu trúc cây Đỏ-Đen.

**Các trường hợp áp dụng**:
- Duy trì việc tìm kiếm giá trị trung vị từ luồng dữ liệu liên tục (sử dụng kết hợp hai cấu trúc multiset hoặc đống heap).
- Truy vấn thống kê thứ tự (tìm phần tử nhỏ thứ $K$ trên tập hợp động liên tục thay đổi).
- Truy vấn theo đoạn (liệt kê các phần tử có giá trị nằm giữa $L$ và $R$).

**Ví dụ: Số trung vị của luồng số (Running Median)** sử dụng hai cấu trúc multiset đóng vai trò đống tối đại (max-heap) bên trái và đống tối tiểu (min-heap) bên phải:

```cpp
void addNum(int num, multiset<int>& left, multiset<int>& right) {
    if (left.empty() || num <= *left.rbegin()) left.insert(num);
    else right.insert(num);
    // Cân bằng kích thước hai tập hợp
    if (left.size() > right.size() + 1) {
        right.insert(*left.rbegin());
        left.erase(prev(left.end()));
    } else if (right.size() > left.size()) {
        left.insert(*right.begin());
        right.erase(right.begin());
    }
}
double findMedian(multiset<int>& left, multiset<int>& right) {
    if (left.size() > right.size()) return *left.rbegin();
    return (*left.rbegin() + *right.begin()) / 2.0;
}
```

**Liên hệ thực tế**: Quản lý bảng điểm thi luôn được sắp xếp ngăn nắp để mỗi khi có học sinh nộp bài thi mới, ta lập tức công bố được điểm trung vị của cả trường một cách nhanh nhất.

## 4. Kỹ thuật Băm cuộn (Rolling Hash – Thuật toán Rabin-Karp)

**Khái niệm**: Kỹ thuật tính toán mã băm cho các cửa sổ trượt trên xâu ký tự trong thời gian $O(1)$ sau khi tốn $O(n)$ bước tiền xử lý ban đầu. Thường dùng trong so khớp chuỗi mẫu.

**Công thức băm đa thức (Polynomial Rolling Hash)**:
$$\text{hash}(s) = \sum_{i=0}^{\text{len}-1} s[i] \cdot p^{\text{len}-1-i} \pmod M$$
Khi dịch chuyển cửa sổ sang phải, ta cập nhật mã băm mới trong $O(1)$:
$$\text{new\_hash} = \left( (\text{old\_hash} - s[\text{left}] \cdot p^{\text{len}-1}) \cdot p + s[\text{right}+1] \right) \pmod M$$

```cpp
#define MOD 1000000007
#define BASE 31

vector<int> rabinKarp(string text, string pattern) {
    int n = text.size(), m = pattern.size();
    if (m > n) return {};
    long long pHash = 0, tHash = 0, pow = 1;
    for (int i = 0; i < m; ++i) {
        pHash = (pHash * BASE + pattern[i]) % MOD;
        tHash = (tHash * BASE + text[i]) % MOD;
        if (i > 0) pow = (pow * BASE) % MOD;
    }
    vector<int> occurrences;
    for (int i = 0; i <= n - m; ++i) {
        if (pHash == tHash) {
            // Kiểm tra lại ký tự thực tế để loại trừ trường hợp xung đột mã băm (false positive)
            if (text.substr(i, m) == pattern) occurrences.push_back(i);
        }
        if (i < n - m) {
            tHash = (tHash - text[i] * pow % MOD + MOD) % MOD;
            tHash = (tHash * BASE + text[i+m]) % MOD;
        }
    }
    return occurrences;
}
```

- **Độ phức tạp thời gian**: Trung bình đạt $O(n + m)$. **Không gian bộ nhớ**: $O(1)$.
- **Liên hệ thực tế**: Tìm kiếm nhanh chóng một mẫu dấu vân tay (pattern) trên một dải giấy dài – bạn chỉ việc so sánh các chữ ký số nhỏ gọn thu được qua mỗi chặng trượt của dải kính, và chỉ kiểm tra chi tiết khi chữ ký số trùng khớp hoàn toàn.

## 5. Thuật toán Bầu chọn Đa số Boyer‑Moore (Boyer‑Moore Majority Vote Algorithm)

**Bài toán**: Tìm phần tử đa số (phần tử xuất hiện nhiều hơn $\lfloor n/2 \rfloor$ lần trong mảng) trong thời gian tối ưu $O(n)$ và chỉ sử dụng bộ nhớ hằng số $O(1)$.

**Thuật toán**:
1. **Lọc ứng viên (Candidate Selection)**: Ghép cặp các phần tử khác nhau và triệt tiêu nhau. Ứng viên còn sống sót cuối cùng là phần tử duy nhất có khả năng làm phần tử đa số.
2. **Xác thực lại (Verification)**: Đếm lại tần suất của ứng viên để bảo đảm tính chính xác (có thể bỏ qua nếu đề bài cam kết chắc chắn tồn tại phần tử đa số).

```cpp
int majorityElement(vector<int>& nums) {
    int candidate = 0, count = 0;
    for (int x : nums) {
        if (count == 0) candidate = x;
        count += (x == candidate) ? 1 : -1;
    }
    // Lượt kiểm tra xác thực lại
    int freq = 0;
    for (int x : nums) if (x == candidate) freq++;
    return (freq > nums.size()/2) ? candidate : -1;
}
```

**Liên hệ thực tế**: Trong một trận chiến đấu tự do, mỗi cặp võ sĩ thuộc các môn phái khác nhau sẽ cùng lao vào triệt tiêu lẫn nhau. Môn phái nào có số lượng võ sĩ đông đảo chiếm hơn nửa tổng số võ sĩ chắc chắn sẽ là phái có người sống sót cuối cùng.

## 6. Lấy mẫu Hồ chứa (Reservoir Sampling)

**Bài toán**: Lựa chọn ngẫu nhiên $k$ phần tử từ một luồng dữ liệu (stream) có độ dài cực lớn hoặc chưa biết trước sao cho mọi phần tử đều có xác suất được chọn hoàn toàn bằng nhau.

**Thuật toán**: Với $i < k$, ta giữ lại toàn bộ $k$ phần tử đầu tiên vào hồ chứa. Với mỗi phần tử thứ $i$ tiếp theo ($i \ge k$), ta sinh ngẫu nhiên chỉ số $j$ trong khoảng $[0..i]$. Nếu $j < k$, ta tiến hành thay thế phần tử tại vị trí $j$ trong hồ chứa bằng phần tử mới này với xác suất bằng $k / (i + 1)$.

```cpp
vector<int> reservoirSample(vector<int>& stream, int k) {
    vector<int> reservoir(k);
    for (int i = 0; i < k; ++i) reservoir[i] = stream[i];
    for (int i = k; i < stream.size(); ++i) {
        int j = rand() % (i + 1);
        if (j < k) reservoir[j] = stream[i];
    }
    return reservoir;
}
```

- **Chứng minh toán học**: Mọi phần tử trong luồng dữ liệu sau khi kết thúc thuật toán đều bảo đảm có xác suất được giữ lại trong hồ chứa bằng chính xác $k / n$.
- **Liên hệ thực tế**: Chọn ngẫu nhiên ra 10 bài hát từ một dịch vụ phát nhạc trực tuyến liên tục bổ sung bài hát mới mà không cần phải tải trước toàn bộ danh sách nhạc về máy – hồ chứa giúp bạn luôn duy trì một tập hợp ngẫu nhiên công bằng nhất.

## 7. Hợp nhất các Khoảng thời gian (Interval Merging)

**Bài toán**: Hợp nhất tất cả các khoảng thời gian bị chồng chéo (overlapping intervals) lên nhau.

**Hướng tiếp cận**: Sắp xếp các khoảng thời gian theo thứ tự thời gian bắt đầu tăng dần. Duyệt qua danh sách và tiến hành gộp nếu thời gian bắt đầu của khoảng hiện tại $\le$ thời gian kết thúc của khoảng đã gộp trước đó.

```cpp
vector<pair<int,int>> mergeIntervals(vector<pair<int,int>>& intervals) {
    if (intervals.empty()) return {};
    sort(intervals.begin(), intervals.end());
    vector<pair<int,int>> merged;
    merged.push_back(intervals[0]);
    for (auto& [start, end] : intervals) {
        if (start <= merged.back().second) {
            merged.back().second = max(merged.back().second, end);
        } else {
            merged.push_back({start, end});
        }
    }
    return merged;
}
```

- **Độ phức tạp thời gian**: $O(n \log n)$. **Không gian bộ nhớ**: $O(n)$.
- **Liên hệ thực tế**: Hợp nhất lịch trình họp bị trùng lặp của bạn trên ứng dụng lịch cá nhân.

## 8. Lập lịch Phòng họp II (Số lượng phòng họp tối thiểu - Meeting Rooms II)

**Bài toán**: Cho danh sách các cuộc họp kèm thời gian bắt đầu và kết thúc, hãy tìm số lượng phòng họp tối thiểu cần chuẩn bị để tổ chức tất cả các cuộc họp mà không bị trùng phòng.

**Phương pháp đường quét (Sweep line)**: Theo dõi lượng cuộc họp diễn ra đồng thời bằng cách tách riêng các mốc thời gian sự kiện: cộng thêm 1 phòng khi bắt đầu (+1) và bớt đi 1 phòng khi kết thúc (-1). Sắp xếp tất cả các sự kiện này lại và cộng dồn lũy kế để tìm giá trị lớn nhất.

```cpp
int minMeetingRooms(vector<pair<int,int>>& intervals) {
    vector<pair<int,int>> events; // (thời gian, loại sự kiện): +1 khi bắt đầu, -1 khi kết thúc
    for (auto& [s, e] : intervals) {
        events.emplace_back(s, 1);
        events.emplace_back(e, -1);
    }
    sort(events.begin(), events.end());
    int curr = 0, maxRooms = 0;
    for (auto& [time, delta] : events) {
        curr += delta;
        maxRooms = max(maxRooms, curr);
    }
    return maxRooms;
}
```

- **Liên hệ thực tế**: Xác định số lượng giảng đường đại học lớn nhất cần chuẩn bị dựa trên thời khóa biểu học của các khoa.

## 9. Thuật toán ngẫu nhiên hóa (Randomized Algorithms)

Thuật toán ngẫu nhiên hóa sử dụng bộ sinh số ngẫu nhiên để đơn giản hóa việc cài đặt hoặc đẩy nhanh tốc độ chạy của chương trình. Chúng thường cho thời gian chạy trung bình cực nhanh và khả năng xảy ra lỗi là vô cùng nhỏ.

### 9.1 Lựa chọn ngẫu nhiên Quickselect (Randomized Selection)

**Bài toán**: Tìm kiếm phần tử nhỏ thứ $K$ trong một mảng số chưa được sắp xếp.

**Hướng tiếp cận**: Chọn ngẫu nhiên một phần tử chốt (pivot), thực hiện phân hoạch mảng tương tự Quick Sort, sau đó chỉ gọi đệ quy thu hẹp vào một nửa chứa phần tử cần tìm.

```cpp
int quickselect(vector<int>& nums, int left, int right, int k) {
    if (left == right) return nums[left];
    int pivotIdx = left + rand() % (right - left + 1);
    swap(nums[pivotIdx], nums[right]);
    int i = left - 1;
    for (int j = left; j < right; ++j)
        if (nums[j] <= nums[right]) swap(nums[++i], nums[j]);
    swap(nums[i+1], nums[right]);
    if (k == i+1) return nums[k];
    else if (k < i+1) return quickselect(nums, left, i, k);
    else return quickselect(nums, i+2, right, k);
}
```

- **Thời gian kỳ vọng**: $O(n)$. **Trường hợp xấu nhất**: $O(n^2)$ nếu chọn phải các điểm chốt cực kỳ tệ (tỷ lệ xảy ra là vô cùng hiếm nhờ tính chất ngẫu nhiên).

### 9.2 Thuật toán Quicksort ngẫu nhiên (Randomized Quicksort)

Hoạt động hoàn toàn tương tự như thuật toán Quicksort thông thường nhưng điểm chốt được chọn ngẫu nhiên hoàn toàn. Việc ngẫu nhiên hóa này giúp loại bỏ triệt để nguy cơ rơi vào trường hợp xấu nhất $O(n^2)$ khi dữ liệu đầu vào đã được sắp xếp sẵn. Thời gian chạy kỳ vọng luôn đạt $O(n \log n)$.

## 10. Bảng tổng hợp các chủ đề nâng cao

| Chuyên đề thuật toán | Ý tưởng cốt lõi | Độ phức tạp thời gian | Độ phức tạp bộ nhớ | Trường hợp áp dụng điển hình |
|-------|-----------|------|-------|----------|
| **Ngăn xếp đơn điệu** | Duy trì thứ tự để tìm phần tử lớn/nhỏ hơn kề cạnh | $O(n)$ | $O(n)$ | Tìm diện tích hình chữ nhật lớn nhất, tìm phần tử lớn hơn kế tiếp |
| **Deque cửa sổ trượt** | Duy trì chỉ số mảng theo thứ tự giảm dần | $O(n)$ | $O(k)$ | Tìm cực đại/cực tiểu trên cửa sổ trượt |
| **Multiset / Ordered set** | Cây tìm kiếm nhị phân cân bằng động | $O(\log n)$ mỗi lượt | $O(n)$ | Tìm trung vị động, truy vấn khoảng giá trị |
| **Băm cuộn (Rabin-Karp)** | Băm đa thức tịnh tiến trên cửa sổ trượt | Trung bình $O(n+m)$ | $O(1)$ | So khớp chuỗi mẫu, phát hiện đạo văn |
| **Bầu chọn đa số Boyer-Moore** | Triệt tiêu lẫn nhau theo từng cặp | $O(n)$ | $O(1)$ | Tìm phần tử xuất hiện nhiều hơn $\lfloor n/2 \rfloor$ |
| **Lấy mẫu hồ chứa** | Thay thế ngẫu nhiên trên luồng dữ liệu | $O(n)$ | $O(k)$ | Chọn ngẫu nhiên mẫu từ nguồn dữ liệu khổng lồ |
| **Hợp nhất khoảng** | Sắp xếp theo điểm đầu và tiến hành gộp | $O(n \log n)$ | $O(n)$ | Hợp nhất lịch trình trùng lặp |
| **Lập lịch phòng họp II** | Đếm sự kiện đồng thời bằng phương pháp đường quét | $O(n \log n)$ | $O(n)$ | Tìm số lượng phòng họp / sân ga tối thiểu |
| **Quickselect** | Phân hoạch ngẫu nhiên tìm cực trị | Kỳ vọng $O(n)$ | $O(\log n)$ | Tìm phần tử nhỏ thứ $K$ |
| **Quicksort ngẫu nhiên** | Chọn chốt ngẫu nhiên để phân hoạch | Kỳ vọng $O(n \log n)$ | $O(\log n)$ | Sắp xếp mảng hiệu năng cực cao |

Các chủ đề nâng cao này là chìa khóa vàng giúp bạn tối ưu hóa giải pháp từ mức độ khá lên mức độ xuất sắc hoàn mỹ trong các kỳ phỏng vấn lập trình. Hãy thực hành tự cài đặt lại từ đầu từng chuyên đề để làm chủ hoàn toàn các kỹ thuật này.
