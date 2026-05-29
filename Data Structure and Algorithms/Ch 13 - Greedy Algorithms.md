# Chương 13: Thuật toán Tham lam (Greedy Algorithms)

Thuật toán Tham lam (Greedy Algorithms) xây dựng lời giải cho bài toán từng bước một bằng cách luôn đưa ra lựa chọn tối ưu cục bộ (phương án có vẻ là tốt nhất tại thời điểm hiện tại) mà không bao giờ thay đổi các quyết định đã đưa ra trong quá khứ. Chương này sẽ trình bày về thuộc tính lựa chọn tham lam, cách nhận diện các mẫu bài toán tham lam, các bài toán kinh điển (như lựa chọn hoạt động, cái túi dạng phân số, lập lịch công việc kèm hạn chót, mã hóa Huffman, bài toán đổi tiền, số lượng sân ga tối thiểu, cùng các thuật toán tìm cây khung tối tiểu MST như Prim và Kruskal) và so sánh chi tiết giữa thuật toán tham lam với quy hoạch động.

## 1. Thuộc tính Lựa chọn Tham lam (Greedy Choice Property)

**Khái niệm**: Một bài toán có thuộc tính lựa chọn tham lam nếu ta có thể đạt được một lời giải tối ưu toàn cục bằng cách thực hiện một chuỗi các lựa chọn tối ưu cục bộ (tham lam).

**Cấu trúc con tối ưu (Optimal Substructure)**: Bài toán có thể phân rã thành các bài toán con nhỏ hơn, và việc kết hợp lựa chọn tham lam tại bước hiện tại với lời giải tối ưu của bài toán con còn lại sẽ tạo nên lời giải tối ưu cho bài toán ban đầu.

**Khi nào nên áp dụng thuật toán tham lam**:
- Bài toán có thể giải quyết bằng một chuỗi các quyết định, mỗi quyết định chọn ra phương án "tốt nhất" khả dĩ tại thời điểm đó.
- Quyết định hiện tại hoàn toàn độc lập và không ảnh hưởng đến tính đúng đắn của các quyết định trong tương lai.
- Bài toán đồng thời thỏa mãn thuộc tính lựa chọn tham lam và cấu trúc con tối ưu.

**Khi nào thuật toán tham lam thất bại**:
- Các bài toán đòi hỏi phải cân nhắc, đánh đổi giữa các bước (ví dụ: bài toán cái túi dạng số nguyên 0/1 Knapsack, tìm đường đi dài nhất).
- Các bài toán có các bài toán con trùng lặp phức tạp cần đến sự hỗ trợ của quy hoạch động (ví dụ: khoảng cách biến đổi edit distance, tìm đường đi ngắn nhất trên đồ thị có cạnh trọng số âm).

**Liên hệ thực tế**: Việc thối tiền thừa với số lượng đồng tiền ít nhất bằng các mệnh giá tiền chuẩn (như 25 xu, 10 xu, 5 xu, 1 xu) – bạn luôn ưu tiên chọn đồng tiền có mệnh giá lớn nhất có thể để thối trước. Cách tiếp cận tham lam này luôn đúng với hệ thống tiền xu của Mỹ hay Việt Nam, nhưng sẽ thất bại với các hệ mệnh giá tiền bất kỳ (ví dụ: nếu có các đồng mệnh giá 1, 3, 4 xu và cần thối 6 xu: thuật toán tham lam sẽ chọn đồng 4 + 1 + 1 = 3 đồng xu, trong khi phương án tối ưu thực sự là chọn 3 + 3 = 2 đồng xu).

## 2. Nhận diện mẫu: Làm thế nào để xác định bài toán Tham lam

Nhận biết một bài toán có thể giải được bằng thuật toán tham lam hay không thường là phần thử thách nhất. Dưới đây là các dấu hiệu định hướng:

### 2.1 Các dấu hiệu nhận biết chính

| Dấu hiệu | Mô tả chi tiết | Ví dụ minh họa |
|-----------|-------------|---------|
| **Sắp xếp + Chọn phương án cục bộ tốt nhất** | Lời giải thường bắt đầu bằng việc sắp xếp dữ liệu đầu vào (theo thời gian bắt đầu, thời gian kết thúc, trọng lượng, giá trị, tỷ lệ,...) và sau đó duyệt qua một lượt duy nhất kèm theo một quy tắc lựa chọn đơn giản. | Lựa chọn hoạt động (sắp xếp theo thời gian kết thúc), lập lịch công việc (sắp xếp theo lợi nhuận). |
| **Yêu cầu tối ưu hóa cực trị** | Đề bài yêu cầu tìm giá trị lớn nhất hoặc nhỏ nhất, nhưng khác với Quy hoạch động, các bài toán con không bị chồng chéo phức tạp lên nhau. | Tìm số lượng sân ga tàu tối thiểu, tối đa hóa lợi nhuận với bài toán cái túi dạng phân số. |
| **Không cần thay đổi quyết định đã đưa ra** | Một khi bạn đưa ra một quyết định chọn lựa, bạn không bao giờ phải quay lại thay đổi nó sau đó. Lựa chọn tham lam hiện tại không làm mất đi tính khả thi của các lựa chọn còn lại tới mức phải quay lui. | Mã hóa Huffman (luôn ghép hai nút có tần suất nhỏ nhất lại với nhau và không bao giờ rã ra). |
| **Khoảng thời gian hoặc phân bổ tài nguyên** | Các bài toán liên quan tới lập lịch trình, các khoảng thời gian (intervals), hoặc phân bổ tài nguyên thường rất dễ có lời giải tham lam. | Lựa chọn hoạt động, số lượng sân ga tối thiểu, lập lịch công việc kèm hạn chót. |
| **Lập luận trao đổi (Exchange Argument) đúng đắn** | Bạn có thể chứng minh mặt toán học rằng: nếu tồn tại một lời giải tối ưu khác với lựa chọn tham lam, ta luôn có thể hoán đổi các phần tử để thu được một lời giải mới tốt bằng hoặc tốt hơn lời giải tối ưu đó. | Bài toán cái túi dạng phân số (hoán đổi các vật phẩm có tỷ lệ giá trị/trọng lượng thấp hơn bằng các vật phẩm có tỷ lệ cao hơn). |

### 2.2 Các câu hỏi tự định hướng

- **Tôi có thể sắp xếp dữ liệu đầu vào rồi duyệt qua một lượt duy nhất không?** Rất nhiều thuật toán tham lam trở nên cực kỳ rõ ràng sau khi dữ liệu được sắp xếp.
- **Bài toán yêu cầu tìm "số lượng lớn nhất" hay "chi phí nhỏ nhất" mà không cần lưu trữ quá nhiều trạng thái phức tạp không?** Nếu kết quả chỉ phụ thuộc vào một vài biến số đơn giản (ví dụ: thời gian kết thúc của hoạt động cuối cùng, sức chứa còn lại), thuật toán tham lam rất có triển vọng.
- **Lựa chọn cục bộ hiện tại chỉ ảnh hưởng trực tiếp đến bài toán con còn lại ngay sau đó chứ không thay đổi cấu trúc của toàn bộ tương lai?** Ví dụ: Trong bài toán lựa chọn hoạt động, việc chọn hoạt động kết thúc sớm nhất sẽ để lại một khoảng thời gian trống dài nhất cho việc xếp các hoạt động tiếp sau đó – nó không làm thay đổi cách thức lựa chọn của các hoạt động sau.
- **Có ví dụ phản chứng nhỏ nào làm sai lệch giải pháp tham lam không?** Hãy tự thiết kế một vài trường hợp nhỏ đặc biệt để thử nghiệm. Nếu thuật toán tham lam thất bại ngay cả trong các trường hợp đơn giản, bạn chắc chắn cần đến Quy hoạch động.

### 2.3 Các mẫu bài toán Tham lam phổ biến (Archetypes)

| Mẫu bài toán | Quy tắc Tham lam điển hình | Các bài toán áp dụng |
|-----------|---------------------|------------------|
| **Thời điểm kết thúc sớm nhất** | Luôn ưu tiên chọn khoảng thời gian / hoạt động có thời điểm kết thúc sớm nhất. | Lựa chọn hoạt động, thuê phòng họp dạy học tối thiểu. |
| **Giá trị đơn vị cao nhất** | Ưu tiên chọn vật phẩm có tỷ lệ giá trị/trọng lượng lớn nhất. | Bài toán cái túi dạng phân số. |
| **Lợi nhuận lớn nhất / Hạn chót muộn nhất** | Xếp các công việc có lợi nhuận cao nhất vào các khe thời gian trống muộn nhất có thể trước hạn chót. | Lập lịch công việc kèm hạn chót (deadlines). |
| **Tần suất thấp nhất trước** | Luôn gộp hai nút có tần suất xuất hiện nhỏ nhất lại với nhau. | Mã hóa Huffman. |
| **Thời gian bắt đầu nhỏ nhất + Quét đường** | Sắp xếp tất cả các sự kiện theo thời gian, dùng một đường quét từ trái qua phải để theo dõi số lượng tài nguyên hoạt động đồng thời lớn nhất. | Tìm số lượng sân ga tối thiểu (dựa vào thời gian đến và đi của các chuyến tàu). |
| **Đồng tiền lớn nhất phù hợp** | Chọn mệnh giá tiền lớn nhất không vượt quá số tiền còn lại cần thối. | Bài toán đổi tiền (chỉ đúng với hệ thống tiền tệ chính tắc). |
| **Cạnh nhỏ nhất đi qua lát cắt** | Luôn chọn cạnh có trọng số nhỏ nhất nối một đỉnh nằm trong cây khung hiện tại với một đỉnh ngoài cây. | Thuật toán Prim tìm cây khung tối tiểu. |
| **Cạnh nhỏ nhất không tạo chu trình** | Sắp xếp tất cả các cạnh theo thứ tự trọng số tăng dần, duyệt chọn cạnh nếu nó không tạo chu trình. | Thuật toán Kruskal tìm cây khung tối tiểu. |

### 2.4 Khi nào thuật toán tham lam đáng nghi ngờ (Cần dùng DP thay thế)

- **Các bài toán con chồng chéo lên nhau rất mạnh** (ví dụ: tìm đường đi ngắn nhất trên đồ thị có trọng số âm, khoảng cách biến đổi xâu).
- **Bài toán liên quan tới "mọi tập con" hoặc "thứ tự lựa chọn cực kỳ quan trọng"** (ví dụ: cái túi 0/1, bài toán người du lịch TSP).
- **Lựa chọn hiện tại có thể cản trở một cách phức tạp sự khả thi của nhiều lựa chọn khác trong tương lai**.
- **Giải pháp tham lam bị sai khi thử nghiệm trên các ví dụ phản chứng nhỏ**.

**Lời khuyên thực tế**: Nếu bạn không chắc chắn 100% về tính đúng đắn của tham lam, hãy thử suy nghĩ giải pháp Quy hoạch động trước, sau đó xem xét liệu có thể tối ưu hóa nó thành thuật toán tham lam bằng cách chứng minh thuộc tính hoán đổi hay không.

## 3. Lựa chọn Hoạt động (Activity Selection)

**Bài toán**: Cho $n$ hoạt động kèm theo thời gian bắt đầu (start) và kết thúc (finish), hãy chọn ra số lượng hoạt động nhiều nhất có thể sao cho không có hai hoạt động nào bị trùng lặp thời gian với nhau.

**Lựa chọn tham lam**: Luôn ưu tiên chọn hoạt động có thời gian kết thúc sớm nhất. Điều này giúp tối đa hóa thời gian trống còn lại cho các hoạt động tiếp theo.

**Các bước thực hiện**:
1. Sắp xếp các hoạt động theo thứ tự thời gian kết thúc tăng dần.
2. Chọn hoạt động đầu tiên.
3. Duyệt qua các hoạt động tiếp theo, nếu thời gian bắt đầu của hoạt động đang xét $\ge$ thời gian kết thúc của hoạt động được chọn gần nhất, ta tiến hành chọn hoạt động đó.

```cpp
struct Activity { int start, finish; };
bool compare(Activity a, Activity b) { return a.finish < b.finish; }

int activitySelection(vector<Activity>& activities) {
    sort(activities.begin(), activities.end(), compare);
    int count = 1;
    int lastFinish = activities[0].finish;
    for (int i = 1; i < activities.size(); ++i) {
        if (activities[i].start >= lastFinish) {
            count++;
            lastFinish = activities[i].finish;
        }
    }
    return count;
}
```

- **Độ phức tạp thời gian**: $O(n \log n)$ do bước sắp xếp. **Không gian phụ trợ**: $O(1)$.
- **Liên hệ thực tế**: Sắp xếp lịch họp cho một phòng hội nghị sao cho tổ chức được nhiều cuộc họp nhất – luôn ưu tiên cuộc họp kết thúc sớm nhất để giải phóng phòng cho cuộc họp tiếp theo.

## 4. Bài toán Cái túi dạng phân số (Fractional Knapsack)

**Bài toán**: Cho các đồ vật có trọng lượng và giá trị tương ứng, và một cái túi có sức chứa tối đa là $W$. Hãy chọn các đồ vật bỏ vào túi sao cho tổng giá trị thu được là lớn nhất, biết rằng bạn có thể chia nhỏ các đồ vật ra một cách tùy ý (chọn một phần của đồ vật).

**Lựa chọn tham lam**: Luôn ưu tiên lấy các đồ vật có tỷ lệ giá trị trên một đơn vị trọng lượng (value/weight ratio) cao nhất trước.

```cpp
struct Item { int value, weight; };
bool compare(Item a, Item b) { 
    return (double)a.value/a.weight > (double)b.value/b.weight; 
}

double fractionalKnapsack(vector<Item>& items, int capacity) {
    sort(items.begin(), items.end(), compare);
    double totalValue = 0.0;
    for (Item& item : items) {
        if (capacity >= item.weight) {
            totalValue += item.value;
            capacity -= item.weight;
        } else {
            totalValue += item.value * ((double)capacity / item.weight);
            break; // Đầy túi
        }
    }
    return totalValue;
}
```

- **Độ phức tạp thời gian**: $O(n \log n)$. **Không gian phụ trợ**: $O(1)$.
- **Tại sao tham lam lại đúng**: Vì chúng ta được phép chia nhỏ đồ vật, nên việc lấp đầy dung tích còn lại của túi bằng đồ vật có tỷ lệ giá trị cao nhất luôn mang lại hiệu quả tối ưu. Điều này hoàn toàn khác biệt với bài toán cái túi số nguyên 0/1 Knapsack nơi thuật toán tham lam bị sai.
- **Liên hệ thực tế**: Bạn có một cái bao tải có sức chứa giới hạn và muốn mang về hỗn hợp các loại bột kim loại quý có giá trị nhất (bột vàng, bột bạc, bột đồng). Bạn sẽ ưu tiên xúc đầy bột vàng trước, sau đó đến bột bạc và cuối cùng là bột đồng cho tới khi đầy bao.

## 5. Lập lịch Công việc kèm Hạn chót (Job Sequencing with Deadlines)

**Bài toán**: Cho danh sách các công việc, mỗi công việc có một hạn chót hoàn thành (deadline) và một mức lợi nhuận (profit) nếu hoàn thành đúng hạn. Mỗi công việc mất đúng 1 đơn vị thời gian để thực hiện và tại mỗi thời điểm chỉ làm được tối đa một công việc. Hãy tìm cách lập lịch để thu được tổng lợi nhuận lớn nhất.

**Lựa chọn tham lam**: Sắp xếp các công việc theo thứ tự lợi nhuận giảm dần. Đối với mỗi công việc, hãy cố gắng xếp lịch thực hiện nó vào thời điểm trống muộn nhất có thể trước hạn chót của công việc đó.

```cpp
struct Job { int id, deadline, profit; };
bool compare(Job a, Job b) { return a.profit > b.profit; }

int jobSequencing(vector<Job>& jobs) {
    sort(jobs.begin(), jobs.end(), compare);
    int maxDeadline = 0;
    for (auto& j : jobs) maxDeadline = max(maxDeadline, j.deadline);
    vector<int> slot(maxDeadline + 1, -1);
    int totalProfit = 0;
    for (Job& j : jobs) {
        for (int t = j.deadline; t > 0; --t) {
            if (slot[t] == -1) {
                slot[t] = j.id;
                totalProfit += j.profit;
                break;
            }
        }
    }
    return totalProfit;
}
```

- **Độ phức tạp thời gian**: $O(n^2)$ trong trường hợp xấu nhất (có thể tối ưu hóa thành $O(n \log n)$ bằng cách sử dụng cấu trúc các tập hợp rời nhau DSU). **Không gian phụ trợ**: $O(\text{maxDeadline})$.
- **Liên hệ thực tế**: Một lập trình viên tự do (freelancer) nhận các dự án có hạn chót và mức thù lao khác nhau; mỗi ngày chỉ có thể làm đúng một dự án, vì vậy anh ta sẽ ưu tiên các dự án trả lương cao nhất và xếp lịch làm chúng sát ngày hết hạn nhất để chừa các khoảng thời gian trống trước đó cho các công việc khác.

## 6. Mã hóa Huffman (Huffman Coding)

**Bài toán**: Cho tần suất xuất hiện của các ký tự trong một văn bản, hãy xây dựng một bộ mã nhị phân không tiền tố (prefix‑free code) sao cho tổng độ dài sau khi mã hóa toàn bộ văn bản là ngắn nhất.

**Lựa chọn tham lam**: Liên tục chọn ra và gộp hai nút có tần suất xuất hiện nhỏ nhất lại với nhau để tạo thành một nút cha mới.

**Các bước thực hiện**:
1. Tạo một nút lá cho mỗi ký tự chứa tần suất của nó.
2. Đưa tất cả các nút lá này vào một hàng đợi ưu tiên (min‑heap).
3. Khi hàng đợi ưu tiên còn nhiều hơn 1 nút:
   - Lấy ra hai nút có tần suất nhỏ nhất.
   - Tạo một nút trung gian mới có tần suất bằng tổng tần suất của hai nút vừa lấy ra.
   - Gán hai nút vừa lấy làm con trái và con phải của nút mới.
   - Đẩy nút trung gian mới này quay trở lại hàng đợi ưu tiên.
4. Nút cuối cùng còn lại duy nhất trong hàng đợi chính là gốc của cây Huffman.

```cpp
struct HuffmanNode {
    char ch;
    int freq;
    HuffmanNode *left, *right;
    HuffmanNode(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
    HuffmanNode(int f, HuffmanNode* l, HuffmanNode* r) : ch('\0'), freq(f), left(l), right(r) {}
};
struct Compare {
    bool operator()(HuffmanNode* a, HuffmanNode* b) { return a->freq > b->freq; }
};

HuffmanNode* buildHuffmanTree(vector<pair<char, int>>& freqTable) {
    priority_queue<HuffmanNode*, vector<HuffmanNode*>, Compare> pq;
    for (auto& p : freqTable)
        pq.push(new HuffmanNode(p.first, p.second));
    while (pq.size() > 1) {
        HuffmanNode* left = pq.top(); pq.pop();
        HuffmanNode* right = pq.top(); pq.pop();
        HuffmanNode* parent = new HuffmanNode(left->freq + right->freq, left, right);
        pq.push(parent);
    }
    return pq.top();
}
```

- **Độ phức tạp thời gian**: $O(n \log n)$ với $n$ là số lượng ký tự phân biệt. **Không gian phụ trợ**: $O(n)$.
- **Liên hệ thực tế**: Tương tự việc gán mã Morse ngắn hơn cho các chữ cái phổ biến (ví dụ chữ 'E' chỉ dùng một dấu chấm `.`) để rút ngắn độ dài của toàn bộ thông điệp truyền đi.

## 7. Bài toán Đổi tiền (Coin Change) - So sánh Tham lam với Quy hoạch động

**Bài toán**: Tìm số lượng đồng tiền ít nhất để cấu thành nên một tổng số tiền cho trước, biết danh sách mệnh giá các đồng tiền.

**Giải pháp tham lam**: Luôn chọn đồng tiền có mệnh giá lớn nhất có thể sao cho mệnh giá đó không vượt quá số tiền còn lại cần đổi.

**Khi nào tham lam đúng**: Khi hệ thống các mệnh giá tiền là *chính tắc* (canonical) (ví dụ hệ thống tiền xu Mỹ: 1, 5, 10, 25). Ví dụ: cần đổi số tiền 30 với các mệnh giá [1, 5, 10, 25] – tham lam sẽ chọn đồng 25 + 5 = 2 đồng tiền (đây là đáp án tối ưu).

**Khi nào tham lam sai**: Khi hệ thống tiền không chính tắc. Ví dụ mệnh giá tiền là [1, 3, 4] và cần đổi số tiền là 6. Giải pháp tham lam sẽ chọn đồng lớn nhất 4 + 1 + 1 = 3 đồng tiền, trong khi lời giải tối ưu thực tế là chọn hai đồng 3 + 3 = 2 đồng tiền. Với những trường hợp này, bắt buộc phải sử dụng Quy hoạch động.

**Cài đặt Quy hoạch động tìm số đồng tiền tối thiểu**:

```cpp
int coinChangeDP(vector<int>& coins, int amount) {
    vector<int> dp(amount+1, amount+1);
    dp[0] = 0;
    for (int i = 1; i <= amount; ++i)
        for (int c : coins)
            if (c <= i)
                dp[i] = min(dp[i], dp[i-c] + 1);
    return dp[amount] > amount ? -1 : dp[amount];
}
```

**Kết luận**: Chỉ nên dùng thuật toán tham lam khi hệ mệnh giá tiền đã được chứng minh là hệ chính tắc; mọi trường hợp khác phải dùng Quy hoạch động để bảo đảm kết quả chính xác nhất.

## 8. Số lượng Sân ga tối thiểu (Minimum Number of Platforms)

**Bài toán**: Cho thời gian đến (arrival) và đi (departure) của các chuyến tàu tại một nhà ga, hãy tìm số lượng sân ga tối thiểu cần thiết để không có chuyến tàu nào phải chờ đợi.

**Giải pháp tham lam**: Sắp xếp độc lập hai danh sách thời gian đến và đi. Sử dụng kỹ thuật hai con trỏ để mô phỏng lượng tàu ra vào nhà ga, đếm số lượng tàu đồng thời có mặt tại ga để tìm ra đỉnh điểm lớn nhất.

```cpp
int findPlatform(vector<int>& arr, vector<int>& dep) {
    sort(arr.begin(), arr.end());
    sort(dep.begin(), dep.end());
    int platforms = 0, maxPlatforms = 0;
    int i = 0, j = 0;
    int n = arr.size();
    while (i < n && j < n) {
        if (arr[i] <= dep[j]) {
            platforms++;
            i++;
            maxPlatforms = max(maxPlatforms, platforms);
        } else {
            platforms--;
            j++;
        }
    }
    return maxPlatforms;
}
```

- **Độ phức tạp thời gian**: $O(n \log n)$. **Không gian phụ trợ**: $O(1)$.
- **Liên hệ thực tế**: Xác định số lượng vị trí đỗ xe đồng thời lớn nhất tại một trung tâm thương mại dựa trên thời gian xe vào và xe ra để thiết kế bãi đỗ xe phù hợp.

## 9. Thuật toán Prim tìm Cây khung tối tiểu (MST)

**Bài toán**: Cho đồ thị vô hướng liên thông có trọng số, hãy tìm một cây khung kết nối tất cả các đỉnh sao cho tổng trọng số các cạnh là nhỏ nhất.

**Lựa chọn tham lam**: Bắt đầu từ một đỉnh bất kỳ, liên tục tìm và thêm vào cây khung cạnh có trọng số nhỏ nhất nối từ một đỉnh nằm trong cây khung hiện tại đến một đỉnh nằm ngoài cây khung.

**Hướng tiếp cận**: Sử dụng hàng đợi ưu tiên (min-heap) để liên tục lấy ra cạnh có chi phí rẻ nhất từ ranh giới của cây khung ra ngoài.

```cpp
using pii = pair<int, int>; // (trọng số, đỉnh)

int primMST(vector<vector<pii>>& adj) {
    int V = adj.size();
    vector<bool> inMST(V, false);
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, 0}); // Bắt đầu từ đỉnh 0
    int totalWeight = 0;
    while (!pq.empty()) {
        auto [w, u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        totalWeight += w;
        for (auto& [v, weight] : adj[u]) {
            if (!inMST[v])
                pq.push({weight, v});
        }
    }
    return totalWeight;
}
```

- **Độ phức tạp thời gian**: $O((V + E) \log V)$ sử dụng đống nhị phân. **Không gian phụ trợ**: $O(V)$.
- **Liên hệ thực tế**: Triển khai mạng lưới đường ống nước kết nối tất cả các hộ dân với tổng chiều dài đường ống ngắn nhất – luôn ưu tiên nối ống từ hộ dân đã có nước sang hộ dân kề cạnh chưa có nước với khoảng cách ngắn nhất.

## 10. Thuật toán Kruskal tìm Cây khung tối tiểu (MST)

**Bài toán**: Tương tự như thuật toán Prim – tìm cây khung tối tiểu của đồ thị vô hướng có trọng số.

**Lựa chọn tham lam**: Sắp xếp tất cả các cạnh của đồ thị theo thứ tự trọng số tăng dần. Lần lượt duyệt qua các cạnh này và chọn thêm cạnh vào cây khung nếu cạnh đó không tạo thành chu trình với các cạnh đã chọn trước đó (sử dụng cấu trúc dữ liệu các tập hợp rời nhau DSU để phát hiện chu trình nhanh chóng).

```cpp
struct DSU {
    vector<int> parent, rank;
    DSU(int n) {
        parent.resize(n); rank.resize(n, 0);
        for (int i = 0; i < n; ++i) parent[i] = i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return false;
        if (rank[rx] < rank[ry]) parent[rx] = ry;
        else if (rank[rx] > rank[ry]) parent[ry] = rx;
        else { parent[ry] = rx; rank[rx]++; }
        return true;
    }
};

int kruskalMST(vector<tuple<int,int,int>>& edges, int V) {
    // edges: (trọng số, u, v)
    sort(edges.begin(), edges.end());
    DSU dsu(V);
    int totalWeight = 0, edgesUsed = 0;
    for (auto& [w, u, v] : edges) {
        if (dsu.unite(u, v)) {
            totalWeight += w;
            edgesUsed++;
            if (edgesUsed == V-1) break;
        }
    }
    return totalWeight;
}
```

- **Độ phức tạp thời gian**: $O(E \log E)$ cho việc sắp xếp các cạnh + $O(E \cdot \alpha(V))$ cho các truy vấn trên DSU. **Không gian phụ trợ**: $O(V)$.
- **Liên hệ thực tế**: Nối các thị trấn lại với nhau bằng cách chọn các con đường rẻ nhất có thể, miễn là con đường mới chọn không tạo thành một vòng lặp kín giữa các thị trấn đã được kết nối thông nhau – các cụm thị trấn sẽ dần liên thông lại thành một mạng lưới duy nhất.

### So sánh giữa thuật toán Prim và Kruskal

| Đặc điểm so sánh | Thuật toán Prim | Thuật toán Kruskal |
|---------|------|---------|
| **Biểu diễn đồ thị** | Danh sách kề (Adjacency list) | Danh sách cạnh (Edge list) |
| **Cấu trúc dữ liệu chính** | Hàng đợi ưu tiên (Min‑heap) | Thuật toán sắp xếp + Cấu trúc DSU |
| **Phù hợp nhất cho** | Đồ thị dày (dense graphs - số cạnh lớn $E \approx V^2$) | Đồ thị thưa (sparse graphs - số cạnh nhỏ $E \approx V$) |
| **Đỉnh bắt đầu** | Chọn một đỉnh xuất phát bất kỳ | Không cần đỉnh xuất phát |
| **Phát hiện chu trình** | Thông qua mảng đánh dấu đỉnh đã duyệt | Thông qua cấu trúc dữ liệu DSU |

## 11. Các trường hợp KHÔNG nên dùng Thuật toán Tham lam

| Bài toán | Tại sao thuật toán Tham lam thất bại | Phương án xử lý tối ưu hơn |
|---------|------------------|-----------------|
| **Bài toán Cái túi 0/1** | Việc chọn vật phẩm có tỷ lệ giá trị/trọng lượng cao nhất có thể làm lãng phí dung tích còn lại của túi do không được phép chia nhỏ vật phẩm | Quy hoạch động |
| **Đường đi dài nhất trên đồ thị DAG** | Việc rẽ vào cạnh có giá trị lớn nhất trước mắt không bảo đảm sẽ dẫn đến chặng đường đi có tổng độ dài dài nhất | Quy hoạch động trên DAG hoặc thuật toán BFS biến thể |
| **Đổi tiền mệnh giá bất kỳ** | Lựa chọn đồng tiền mệnh giá lớn nhất có thể lấy đi cơ hội kết hợp các đồng tiền nhỏ hơn để tạo nên số lượng đồng tiền tối ưu | Quy hoạch động |
| **Tìm chặng đi ngắn nhất với bước nhảy biến đổi** | Không phải cứ bước nhảy cục bộ ngắn nhất lúc ban đầu luôn dẫn tới tổng quãng đường đi ngắn nhất về sau | Quy hoạch động hoặc Dijkstra |

## 12. Bảng tổng hợp các bài toán Tham lam

| Bài toán | Lựa chọn Tham lam cục bộ | Độ phức tạp thời gian | Ghi chú |
|---------|---------------|-----------------|-------|
| **Lựa chọn Hoạt động** | Hoạt động có thời gian kết thúc sớm nhất | $O(n \log n)$ | Tối ưu cho các khoảng thời gian không có trọng số |
| **Cái túi dạng phân số** | Vật phẩm có giá trị trên một đơn vị trọng lượng lớn nhất | $O(n \log n)$ | Cho phép chia nhỏ vật phẩm tùy ý |
| **Lập lịch Công việc** | Công việc có lợi nhuận cao nhất, xếp lịch muộn nhất có thể trước deadline | $O(n^2)$ hoặc $O(n \log n)$ với DSU | Các công việc có thời gian thực hiện bằng nhau |
| **Mã hóa Huffman** | Gộp hai nút có tần suất xuất hiện nhỏ nhất | $O(n \log n)$ | Tạo ra mã tiền tố tối ưu nhất |
| **Đổi tiền (chính tắc)** | Chọn mệnh giá tiền lớn nhất $\le$ số tiền còn lại cần thối | $O(n \log n)$ nếu mệnh giá chưa xếp | Thất bại nếu hệ tiền không chính tắc |
| **Số lượng Sân ga** | Dùng đường quét đếm số lượng tàu đồng thời trùng lặp thời gian | $O(n \log n)$ | Đếm số lượng trùng nhau lớn nhất |
| **Thuật toán Prim MST** | Chọn cạnh nhỏ nhất nối từ cây khung hiện tại ra ngoài | $O((V+E) \log V)$ | Phù hợp nhất cho đồ thị dày |
| **Thuật toán Kruskal MST** | Chọn cạnh nhỏ nhất không tạo thành chu trình | $O(E \log E)$ | Phù hợp nhất cho đồ thị thưa + sử dụng DSU |

Chương tiếp theo sẽ bao gồm các kiến thức nâng cao rất thú vị khác: Thuật toán Chia để trị (Divide and Conquer - định lý thợ Master Theorem, bài toán cặp điểm gần nhất Closest Pair, nhân ma trận Strassen) cùng các mô hình thiết kế thuật toán liên quan.
