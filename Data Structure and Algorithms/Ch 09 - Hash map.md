# Chương 9: Bảng băm (Hash map)

Cấu trúc Bảng băm cung cấp các thao tác chèn, xóa và tìm kiếm phần tử với tốc độ thời gian tiệm cận hằng số trung bình đạt $O(1)$ bằng cách ánh xạ các khóa dữ liệu (keys) thành các chỉ số mảng (indices) thông qua một Hàm băm (Hash function). Chương học này sẽ trình bày chi tiết về hàm băm, các chiến lược xử lý xung đột băm, hệ số tải (load factor), kỹ thuật tái băm (rehashing), cách cài đặt cấu trúc Hash Map, Hash Set và các ứng dụng kinh điển của chúng.

---

## 1. Hàm băm (Hash Functions)

**Bản chất (What)**: Một hàm số thực hiện ánh xạ một khóa đầu vào (có thể là số nguyên, chuỗi ký tự hoặc đối tượng bất kỳ) thành một chỉ số nguyên nằm trong khoảng giới hạn $[0, m-1]$.

**Các đặc tính tối ưu cần có**:
- **Tính xác định (Deterministic)**: Cùng một khóa đầu vào bắt buộc luôn tạo ra cùng một giá trị băm duy nhất.
- **Tính toán cực nhanh**: Đạt độ phức tạp thời gian $O(1)$ hoặc $O(\text{độ_dài_khóa})$.
- **Phân bố đồng đều (Uniform distribution)**: Giảm thiểu tối đa xung đột băm bằng cách phân tán các khóa băm đồng đều trên toàn bộ mảng lưu trữ.
- **Hạn chế đụng độ**: Hai khóa khác nhau rất hiếm khi bị ánh xạ vào chung một chỉ số.

**Các ví dụ hàm băm đơn giản**:
- Đối với khóa số nguyên: $\text{hash}(key) = key \pmod m$ (Thường ưu tiên chọn kích thước mảng $m$ là một số nguyên tố lớn để cải thiện hiệu quả phân bố dư).
- Đối với khóa chuỗi ký tự: Áp dụng phương pháp băm cuốn đa thức (polynomial rolling hash): $\text{hash} = (\text{hash} \cdot \text{BASE} + c) \pmod m$.

```cpp
// Hàm băm số nguyên đơn giản
int hashInt(int key, int m) { 
    return key % m; 
}

// Hàm băm cuốn đa thức dành cho chuỗi ký tự
int hashString(const string& key, int m) {
    const int BASE = 31;
    long long hash = 0;
    for (char c : key) {
        hash = (hash * BASE + c) % m;
    }
    return (int)hash;
}
```

**Phép so sánh trong thế giới thực**: Phân loại sách trong thư viện. Tiêu đề của cuốn sách (khóa) được ánh xạ vào một số thứ tự kệ sách cụ thể (chỉ số băm). Một quy chuẩn phân loại tốt sẽ giúp phân bổ lượng sách đồng đều trên tất cả các kệ, tránh tình trạng kệ thì quá tải, kệ thì trống rỗng.

---

## 2. Giải quyết Xung đột băm (Collision Resolution)

Xung đột băm (đụng độ băm) xảy ra khi hai khóa hoàn toàn khác nhau cùng bị hàm băm ánh xạ về chung một chỉ số nguyên của mảng.

### 2.1 Phương pháp kết xích (Separate Chaining)

**Bản chất (What)**: Mỗi ô chỉ mục (bucket) của mảng băm sẽ quản lý một danh sách liên kết (hoặc một mảng động) chứa tất cả các cặp khóa-giá trị cùng bị ánh xạ về chỉ số đó.

**Các thao tác cơ bản**:
- **Chèn**: Tính chỉ số băm, thêm nút mới vào đầu/cuối danh sách liên kết tương ứng (có thể kiểm tra trùng lặp trước).
- **Tìm kiếm**: Tính chỉ số băm, duyệt qua danh sách liên kết để đối chiếu khóa.
- **Xóa**: Tính chỉ số băm, xóa nút khỏi danh sách liên kết.

**Phân tích hiệu năng**:
- Trường hợp trung bình: Đạt độ phức tạp $O(1 + \alpha)$ với $\alpha = n/m$ là hệ số tải (load factor).
- Trường hợp xấu nhất (tất cả các khóa cùng bị đụng độ rơi vào duy nhất một danh sách): Suy thoái thành $O(n)$.

```cpp
template<typename K, typename V>
class HashMapChaining {
    struct Node { 
        K key; 
        V value; 
        Node* next; 
    };
    vector<Node*> buckets;
    int size, capacity;
    int hash(K key) { 
        return key % capacity; 
    }
public:
    HashMapChaining(int cap = 101) : capacity(cap), size(0) {
        buckets.resize(capacity, nullptr);
    }
    void insert(K key, V value) {
        int idx = hash(key);
        Node* curr = buckets[idx];
        while (curr) {
            if (curr->key == key) { 
                curr->value = value; // Cập nhật nếu khóa đã tồn tại
                return; 
            }
            curr = curr->next;
        }
        Node* newNode = new Node{key, value, buckets[idx]};
        buckets[idx] = newNode;
        size++;
    }
    bool find(K key, V& out) {
        int idx = hash(key);
        Node* curr = buckets[idx];
        while (curr) {
            if (curr->key == key) { 
                out = curr->value; 
                return true; 
            }
            curr = curr->next;
        }
        return false;
    }
};
```
- **Khi nào nên áp dụng**: Đây là giải pháp xử lý xung đột phổ biến nhất; hoạt động rất ổn định ngay cả khi hệ số tải $\alpha$ vượt quá 1.

---

### 2.2 Phương pháp địa chỉ mở (Open Addressing)

**Bản chất (What)**: Toàn bộ các phần tử được lưu trữ trực tiếp ngay trong các ô của mảng băm. Khi xảy ra đụng độ, thuật toán sẽ tiến hành dò tìm (probing) các ô trống kế cận trong mảng theo một quy luật xác định để điền phần tử mới.

**Các chiến lược dò tìm phổ biến**:
- **Dò tuyến tính (Linear probing)**: Công thức dò ô $\text{index} = (\text{hash} + i) \pmod m$ với $i = 0, 1, 2, \dots$
  - *Ưu điểm:* Cực kỳ thân thiện với bộ nhớ đệm (cache friendly) vì các phần tử nằm liên tục.
  - *Nhược điểm:* Hiện tượng tụ cụm sơ cấp (primary clustering)—tạo ra các dải ô bị chiếm đóng liên tục dài, làm tăng chi phí dò tìm.
- **Dò bậc hai (Quadratic probing)**: Công thức dò ô $\text{index} = (\text{hash} + c_1 \cdot i + c_2 \cdot i^2) \pmod m$.
  - Hạn chế được tụ cụm sơ cấp nhưng có thể không dò qua được toàn bộ các ô trống trong mảng.
- **Băm kép (Double hashing)**: Công thức dò $\text{index} = (\text{hash}_1 + i \cdot \text{hash}_2(key)) \pmod m$, sử dụng một hàm băm thứ hai độc lập.
  - Mang lại khả năng phân bố đều và tránh hiện tượng tụ cụm tối ưu nhất.

**Phân tích hiệu năng**: Cực kỳ nhạy cảm với hệ số tải; trong thực tế cần duy trì cờ giới hạn $\alpha < 0.7$ để tránh việc suy giảm tốc độ.

```cpp
class HashMapOpenAddressing {
    vector<pair<int, int>> table;
    vector<bool> occupied, deleted; // Cờ deleted phục vụ cơ chế xóa lười (lazy deletion)
    int capacity, size;
    int hash1(int key) { return key % capacity; }
    int hash2(int key) { return 1 + (key % (capacity - 1)); }
public:
    HashMapOpenAddressing(int cap = 101) : capacity(cap), size(0) {
        table.resize(capacity);
        occupied.resize(capacity, false);
        deleted.resize(capacity, false);
    }
    void insert(int key, int value) {
        int idx = hash1(key);
        int step = hash2(key);
        for (int i = 0; i < capacity; ++i) {
            int probe = (idx + i * step) % capacity;
            if (!occupied[probe] || deleted[probe]) {
                table[probe] = {key, value};
                occupied[probe] = true;
                deleted[probe] = false;
                size++;
                return;
            }
            if (occupied[probe] && table[probe].first == key) {
                table[probe].second = value; // Cập nhật
                return;
            }
        }
        // Bảng đầy – cần kích hoạt cơ chế Tái băm (Rehash)
    }
};
```

---

## 3. Hệ số tải và Kỹ thuật Tái băm (Load Factor and Rehashing)

**Hệ số tải (Load factor)** $\alpha$ là tỷ số giữa số lượng phần tử hiện tại $n$ và kích thước mảng băm $m$:
$$\alpha = \frac{n}{m}$$

- Đối với phương pháp kết xích, $\alpha > 1$ hoàn toàn được chấp nhận (chiều dài trung bình của các liên kết = $\alpha$).
- Đối với phương pháp địa chỉ mở, bắt buộc phải khống chế $\alpha < 0.7$ để ngăn chặn chi phí dò tìm tăng đột biến.

**Tái băm (Rehashing)**: Khi hệ số tải vượt ngưỡng cờ quy định (ví dụ: $\alpha \ge 0.75$), ta tiến hành cấp phát một mảng băm mới có kích thước gấp đôi, tính toán lại giá trị băm cho toàn bộ các phần tử cũ và chèn chúng vào mảng mới.

```cpp
void rehash() {
    int newCapacity = capacity * 2;
    vector<pair<int, int>> oldTable = table;
    vector<bool> oldOccupied = occupied;
    capacity = newCapacity;
    table.assign(capacity, {0,0});
    occupied.assign(capacity, false);
    deleted.assign(capacity, false);
    size = 0;
    for (int i = 0; i < oldTable.size(); ++i) {
        if (oldOccupied[i] && !deleted[i]) {
            insert(oldTable[i].first, oldTable[i].second);
        }
    }
}
```

---

## 4. Cài đặt Hash Map và Hash Set

### 4.1 Bản đồ băm (Hash Map - Từ điển)
Cấu trúc lưu trữ thông tin dưới dạng các cặp Khóa - Giá trị (Key-Value). Trong thư viện chuẩn C++, cấu trúc này được cung cấp qua lớp `std::unordered_map` (các thao tác đạt độ phức tạp trung bình $O(1)$).

### 4.2 Tập hợp băm (Hash Set)
Cấu trúc chỉ lưu trữ duy nhất các Khóa (không lưu giá trị kèm theo). Trong C++ là lớp `std::unordered_set`.

**Ví dụ: Tự cài đặt cấu trúc Hash Set đơn giản bằng phương pháp kết xích**:
```cpp
class HashSet {
    vector<list<int>> buckets;
    int hash(int key) { 
        return key % buckets.size(); 
    }
public:
    HashSet(int cap = 101) : buckets(cap) {}
    void add(int key) {
        int idx = hash(key);
        for (int x : buckets[idx]) {
            if (x == key) return; // Tránh trùng lặp
        }
        buckets[idx].push_back(key);
    }
    bool contains(int key) {
        int idx = hash(key);
        for (int x : buckets[idx]) {
            if (x == key) return true;
        }
        return false;
    }
    void remove(int key) {
        int idx = hash(key);
        buckets[idx].remove(key);
    }
};
```

---

## 5. Các ứng dụng thực tế kinh điển của Bảng băm

### 5.1 Bài toán tổng hai số (Two‑Sum Problem)

**Bài toán**: Cho một mảng số nguyên và giá trị `target`, hãy trả về chỉ số index của hai số có tổng bằng đúng `target`.

**Ý tưởng**: Sử dụng một bảng băm để lưu trữ ánh xạ từ Giá trị sang Chỉ số. Khi duyệt qua mỗi phần tử `nums[i]`, ta kiểm tra xem giá trị phần bù `target - nums[i]` đã xuất hiện trong bảng băm hay chưa.

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> m; // dạng: giá_trị -> chỉ_số
    for (int i = 0; i < nums.size(); ++i) {
        int complement = target - nums[i];
        if (m.find(complement) != m.end()) {
            return {m[complement], i};
        }
        m[nums[i]] = i;
    }
    return {};
}
```
- **Độ phức tạp thời gian**: $O(n)$
- **Độ phức tạp không gian**: $O(n)$

### 5.2 Tìm mảng con có tổng bằng K (Subarray Sum Equals K)

**Bài toán**: Đếm số lượng mảng con liên tục có tổng các phần tử đúng bằng giá trị $k$.

**Ý tưởng**: Tính toán tổng tiền tố lũy kế (prefix sum) kết hợp một bảng băm lưu tần suất xuất hiện của các tổng tiền tố trước đó. Tại mỗi phần tử có tổng tiền tố `prefix`, ta cộng dồn số lượng xuất hiện của phần bù `prefix - k` đã lưu giữ.

```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> freq;
    freq[0] = 1; // Khởi tạo tổng tiền tố rỗng
    int prefix = 0, count = 0;
    for (int x : nums) {
        prefix += x;
        count += freq[prefix - k];
        freq[prefix]++;
    }
    return count;
}
```
- **Độ phức tạp thời gian**: $O(n)$
- **Độ phức tạp không gian**: $O(n)$

### 5.3 Tìm chuỗi số liên tục dài nhất (Longest Consecutive Sequence)

**Bài toán**: Tìm chiều dài của chuỗi các phần tử liên tục dài nhất từ một mảng số chưa được sắp xếp (yêu cầu thời gian tuyến tính $O(n)$).

**Ý tưởng**: Đưa toàn bộ các phần tử vào một tập hợp băm Hash Set. Duyệt qua từng số, nếu số đứng trước nó (`num-1`) không tồn tại trong tập hợp băm, chứng tỏ số hiện tại là điểm bắt đầu của một chuỗi liên tục tiềm năng. Ta tiến hành đếm tăng dần các số liên tục tiếp theo từ điểm mấu chốt này.

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> s(nums.begin(), nums.end());
    int best = 0;
    for (int x : nums) {
        // Chỉ bắt đầu đếm nếu x là phần tử nhỏ nhất của chuỗi tiềm năng
        if (s.find(x - 1) == s.end()) {
            int len = 1;
            while (s.find(x + len) != s.end()) {
                len++;
            }
            best = max(best, len);
        }
    }
    return best;
}
```
- **Độ phức tạp thời gian**: $O(n)$ vì mỗi phần tử chỉ bị duyệt kiểm tra tối đa 2 lần.

### 5.4 Gom nhóm các chuỗi hoán vị (Group Anagrams)

**Bài toán**: Gom nhóm các chuỗi ký tự là hoán vị của nhau từ một danh sách các chuỗi đầu vào.

**Ý tưởng**: Với mỗi chuỗi, ta thực hiện sắp xếp các ký tự cấu thành để tạo ra một "khóa chuẩn" chung (hoặc đếm tần suất chữ cái làm khóa), sau đó đưa chúng vào Hash Map dưới dạng cờ nhóm.

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    for (string s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        groups[key].push_back(s);
    }
    vector<vector<string>> result;
    for (auto& p : groups) {
        result.push_back(p.second);
    }
    return result;
}
```
- **Độ phức tạp thời gian**: $O(n \cdot k \log k)$ với $k$ là độ dài lớn nhất của một từ.

### 5.5 Thiết kế cơ chế bộ nhớ đệm LRU Cache (Hash Map + Danh sách liên kết kép)

**Bài toán**: Thiết kế bộ nhớ đệm LRU (Least Recently Used) hỗ trợ tự động giải phóng phần tử ít được sử dụng nhất khi vượt quá dung lượng.

**Ý tưởng thiết kế kết hợp hoàn hảo**:
- **Bảng băm (Hash Map)**: Ánh xạ trực tiếp từ Khóa sang con trỏ chỉ tới Nút của danh sách liên kết kép để đạt thời gian truy xuất phần tử cực nhanh $O(1)$.
- **Danh sách liên kết kép (Doubly Linked List)**: Lưu giữ các nút theo trật tự thời gian truy xuất gần nhất (phần tử mới dùng ở đầu danh sách, phần tử lâu không dùng ở cuối).

```cpp
class LRUCache {
    struct Node {
        int key, val;
        Node *prev, *next;
        Node(int k, int v) : key(k), val(v), prev(nullptr), next(nullptr) {}
    };
    unordered_map<int, Node*> m;
    Node *head, *tail;
    int cap, size;

    void addToHead(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }
    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
    void moveToHead(Node* node) {
        removeNode(node);
        addToHead(node);
    }
    Node* removeTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }
public:
    LRUCache(int capacity) : cap(capacity), size(0) {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->next = tail;
        tail->prev = head;
    }
    int get(int key) {
        if (m.find(key) == m.end()) return -1;
        Node* node = m[key];
        moveToHead(node);
        return node->val;
    }
    void put(int key, int value) {
        if (m.find(key) != m.end()) {
            Node* node = m[key];
            node->val = value;
            moveToHead(node);
            return;
        }
        Node* newNode = new Node(key, value);
        m[key] = newNode;
        addToHead(newNode);
        size++;
        if (size > cap) {
            Node* toRemove = removeTail();
            m.erase(toRemove->key);
            delete toRemove;
            size--;
        }
    }
};
```
- **Độ phức tạp**: Đạt thời gian hằng số tuyệt đối $O(1)$ cho cả hai thao tác đọc dữ liệu `get` và ghi dữ liệu `put`.

**Phép so sánh trong thế giới thực**: Thủ thư luôn giữ các cuốn sách được mượn thường xuyên nhất ở kệ sách ngay cạnh quầy phục vụ (những cuốn vừa dùng). Khi sách mới chuyển về và kệ hết chỗ, cuốn sách nằm ở cuối kệ (cuốn đã lâu không ai mượn) sẽ bị chuyển vào kho lưu trữ.

---

## 6. Bảng tổng hợp các khái niệm cốt lõi

| Khái niệm học thuật | Đặc điểm & Nhận thức mấu chốt |
| :--- | :--- |
| **Hàm băm** | Thực hiện chuyển đổi khóa thành chỉ số mảng; phân bố đồng đều là yếu tố sống còn để giảm đụng độ. |
| **Separate Chaining** | Đơn giản, an toàn kể cả khi hệ số tải $\alpha > 1$; mỗi bucket là một danh sách liên kết. |
| **Open Addressing** | Lưu trực tiếp trên mảng băm; sử dụng các chuỗi dò tìm (tuyến tính, bậc hai, băm kép) để tìm ô trống. |
| **Hệ số tải** | Công thức $\alpha = n/m$; cần kích hoạt tái băm khi vượt quá ngưỡng (thông thường là 0.75). |
| **Hai số có tổng bằng K** | Sử dụng Hash Map để lưu giữ các giá trị và chỉ số index đã duyệt qua. |
| **Mảng con có tổng bằng K** | Sử dụng mảng cộng dồn tổng tiền tố kết hợp bảng băm lưu tần suất. |
| **Chuỗi liên tục dài nhất** | Tận dụng Hash Set để tìm kiếm phần tử nhỏ nhất và kiểm tra láng giềng trong $O(n)$. |
| **Gom nhóm hoán vị** | Sắp xếp các chữ cái để tạo ra khóa băm chung đại diện cho cả nhóm. |
| **LRU Cache** | Sự kết hợp hoàn hảo giữa Hash Map (truy xuất nhanh) và Danh sách kép (duy trì trật tự thời gian). |

Chương tiếp theo sẽ đi sâu vào cấu trúc dữ liệu **Cây (Trees)** (Cây nhị phân, cây BST cân bằng, các phương pháp duyệt cây...).
