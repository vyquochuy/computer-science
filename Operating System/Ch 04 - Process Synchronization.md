# Chương 4: Đồng bộ hóa Tiến trình (Process Synchronization)

Khi nhiều tiến trình hoặc luồng cùng truy cập và thao tác đồng thời trên một vùng dữ liệu chia sẻ, kết quả thực thi có thể trở nên không đồng nhất hoặc sai lệch hoàn toàn. Đồng bộ hóa tiến trình (*Process synchronization*) đảm bảo các tiến trình hợp tác hoạt động theo cách có kiểm soát, ngăn chặn hiện tượng tranh chấp dữ liệu (race conditions) và duy trì tính nhất quán dữ liệu của toàn hệ thống. Chương này bao gồm các bài toán kinh điển, giải pháp lý thuyết và các công cụ cấp hệ thống dùng để điều phối truy cập tài nguyên dùng chung.

---

## Vấn đề Miền Găng (Critical Section Problem)

Một **miền găng** (*critical section*) là một đoạn mã nguồn thực hiện các thao tác truy cập vào các biến hoặc tài nguyên chia sẻ (ví dụ: một tệp tin, một biến đếm toàn cục) mà tại một thời điểm chỉ cho phép tối đa một tiến trình hoặc luồng được phép thực thi.

**Vấn đề miền găng** đặt ra yêu cầu thiết kế một giao thức phối hợp sao cho khi một tiến trình đang thực thi bên trong miền găng của nó, không có bất kỳ tiến trình nào khác được phép bước vào miền găng của riêng chúng để truy cập cùng một tài nguyên chia sẻ đó.

**Cấu trúc mã nguồn điển hình của một tiến trình** (chứa miền găng):

```c
do {
    // Vùng vào (Entry section) – xin phép được vào miền găng
    // Miền găng (Critical section) – truy cập các biến chia sẻ
    // Vùng ra (Exit section) – giải phóng quyền truy cập
    // Miền còn lại (Remainder section) – các công việc không găng khác
} while (true);
```

**So sánh thực tế**: Buồng rút tiền ATM (*miền găng*). Tại một thời điểm chỉ có tối đa một người được phép đứng rút tiền tại cây ATM. Những người đang đứng xếp hàng chờ (*các tiến trình*) phải phối hợp trật tự – nếu hai người cùng lúc cố tình nhấn nút thao tác trên máy, hệ thống có thể nhả tiền sai hoặc làm lỗi số dư tài khoản.

---

## Các Yêu cầu đối với Giải pháp miền găng

Một giải pháp hợp lệ cho vấn đề miền găng bắt buộc phải thỏa mãn đồng thời **ba điều kiện** sau:

| Yêu cầu | Ý nghĩa kỹ thuật |
| :--- | :--- |
| **Loại trừ tương hỗ (Mutual exclusion)** | Nếu tiến trình $P_i$ đang thực thi trong miền găng của nó, không có tiến trình nào khác được phép thực thi trong miền găng tương ứng. |
| **Tiến triển (Progress)** | Nếu không có tiến trình nào đang trong miền găng và có một số tiến trình đang muốn vào, chỉ những tiến trình không nằm trong miền còn lại (remainder section) mới được tham gia quyết định tiến trình nào sẽ được vào miền găng tiếp theo. Quyết định lựa chọn này không được phép trì hoãn vô hạn. |
| **Chờ đợi có giới hạn (Bounded waiting)** | Sau khi một tiến trình gửi yêu cầu xin vào miền găng của nó, phải tồn tại một giới hạn (hạn định) về số lần các tiến trình khác được phép vượt mặt để vào miền găng trước khi yêu cầu của tiến trình này được chấp nhận. Nhằm tránh hiện tượng đói tài nguyên (starvation). |

---

## Giải pháp Phần mềm: Thuật toán Peterson

Thuật toán Peterson (phát minh năm 1981) là giải pháp thuần phần mềm kinh điển giải quyết vấn đề miền găng cho **hai tiến trình** ($P_0$ và $P_1$) thỏa mãn đầy đủ cả ba yêu cầu trên, hoạt động ngay cả khi phần cứng không hỗ trợ các lệnh nguyên tử.

**Các cấu trúc dữ liệu dùng chung**:
- `int turn;` – Biến chỉ ra lượt của tiến trình nào được phép vào miền găng.
- `bool flag[2];` – Mảng logic, trong đó `flag[i] = true` biểu thị tiến trình $i$ đã sẵn sàng vào miền găng.

**Mã nguồn thuật toán cho tiến trình $i$** (với tiến trình còn lại là $j = 1 - i$):

```c
do {
    flag[i] = true;           // Tôi muốn vào miền găng
    turn = j;                 // Nhường lượt cho tiến trình kia chạy trước (lịch sự)
    while (flag[j] && turn == j);  // Chờ đợi vòng lặp (busy wait) nếu tiến trình kia đã sẵn sàng và đang là lượt của nó

    // --- MIỀN GĂNG (CRITICAL SECTION) ---

    flag[i] = false;          // Tôi đã xong việc và đi ra

    // --- MIỀN CÒN LẠI (REMAINDER SECTION) ---
} while (true);
```

**Tại sao thuật toán hoạt động chính xác**:
- **Loại trừ tương hỗ**: Nếu cả hai tiến trình cùng muốn vào, biến `turn` chỉ có thể nhận giá trị bằng 0 hoặc 1 tại một thời điểm, do đó chỉ có duy nhất một tiến trình vượt qua được vòng lặp `while`.
- **Tiến triển**: Nếu tiến trình $j$ đang ở miền còn lại, giá trị `flag[j] = false`, do đó tiến trình $i$ sẽ lập tức vượt qua vòng lặp `while` để vào miền găng mà không bị cản trở.
- **Chờ đợi có giới hạn**: Tiến trình $i$ chỉ phải chờ tối đa một lượt của tiến trình $j$ trước khi được nhường quyền truy cập.

**So sánh thực tế**: Hai người cùng chia sẻ một bốt điện thoại công cộng duy nhất. Họ cùng giơ tay lên biểu thị muốn sử dụng bốt (`flag[i]=true`) và lịch sự nói với người kia: "Bạn đi trước đi" (`turn = j`). Nếu cả hai người cùng giơ tay, người nào được nhường lượt (dựa trên lời nói "bạn trước" cuối cùng được ghi nhận) sẽ lùi lại và nhường người kia bước vào bốt điện thoại.

> **Lưu ý**: Thuật toán Peterson rất thanh lịch về mặt lý thuyết sư phạm, nhưng nó sẽ **không hoạt động chính xác** trên các kiến trúc CPU đa nhân hiện đại có cơ chế thực thi lệnh ngoài tuần tự (out-of-order execution) trừ khi sử dụng thêm các rào cản bộ nhớ (memory barriers).

---

## Hỗ trợ Phần cứng: Lệnh Test‑and‑Set và Compare‑and‑Swap

Các bộ vi xử lý hiện đại cung cấp các lệnh phần cứng nguyên tử (*atomic instructions*) đặc biệt cho phép đọc, sửa đổi và ghi lại dữ liệu vào bộ nhớ chỉ trong một bước duy nhất không thể bị ngắt. Các lệnh này được sử dụng để xây dựng các công cụ đồng bộ hóa hiệu năng cao.

### Lệnh Test‑and‑Set (TAS)
Thực hiện đọc nguyên tử một vị trí bộ nhớ và tự động ghi giá trị `true` (hoặc 1) vào vị trí đó.

**Định nghĩa logic (Mô phỏng nguyên tử)**:
```c
bool test_and_set(bool *target) {
    bool old = *target;
    *target = true;
    return old;
}
```

**Giải pháp miền găng sử dụng TAS** (biến chia sẻ `lock` ban đầu bằng `false`):
```c
do {
    while (test_and_set(&lock));  // Vòng lặp chờ bận (busy wait) cho đến khi lock = false
    
    // --- MIỀN GĂNG (CRITICAL SECTION) ---
    
    lock = false;                 // Giải phóng khóa
    
    // --- MIỀN CÒN LẠI (REMAINDER SECTION) ---
} while (true);
```

### Lệnh Compare‑and‑Swap (CAS)
Thực hiện so sánh nguyên tử giá trị của một biến với một giá trị kỳ vọng; nếu khớp, nó sẽ thay thế bằng giá trị mới.

**Định nghĩa logic (Mô phỏng nguyên tử)**:
```c
int compare_and_swap(int *value, int expected, int new_value) {
    int old = *value;
    if (old == expected)
        *value = new_value;
    return old;
}
```

**Sử dụng CAS cho miền găng**:
```c
do {
    while (compare_and_swap(&lock, 0, 1) != 0); // Chờ cho đến khi lock trở về 0 để thiết lập lên 1
    
    // --- MIỀN GĂNG (CRITICAL SECTION) ---
    
    lock = 0;                     // Giải phóng khóa
    
    // --- MIỀN CÒN LẠI (REMAINDER SECTION) ---
} while (true);
```

**So sánh thực tế**: Chìa khóa nhà vệ sinh công cộng. Lệnh TAS giống như việc bạn chạy đến chộp lấy chiếc chìa khóa trên móc và đồng thời treo ngay biển hiệu "đã lấy" chỉ trong một động tác duy nhất. Lệnh CAS tương tự như việc bạn kiểm tra: "Nếu chiếc chìa khóa đang ở trên móc, tôi sẽ lấy đi và treo biển 'đang bận'; còn nếu chìa khóa không ở đó, tôi sẽ không làm gì cả."

---

## Khóa loại trừ tương hỗ Mutex và Khóa xoay Spinlock

Khóa **Mutex** (*MUTual EXclusion*) là cơ chế khóa đơn giản hỗ trợ hai thao tác nguyên tử là `acquire()` (nhận khóa) và `release()` (giải phóng khóa).

- **Khóa Mutex (Chặn - Blocking)**: Nếu một luồng cố gắng nhận khóa mà khóa đang bị giữ bởi luồng khác, hệ điều hành sẽ đưa luồng yêu cầu vào trạng thái ngủ (chuyển đổi ngữ cảnh để nhường CPU cho tác vụ khác). Phù hợp cho các miền găng có thời gian xử lý dài.
- **Khóa xoay (Spinlock)**: Nếu luồng không thể nhận khóa, nó sẽ liên tục chạy vòng lặp kiểm tra trạng thái khóa (xoay tròn - spinning) cho đến khi khóa được mở, tiêu tốn chu kỳ xử lý của CPU. Phù hợp cho các miền găng siêu ngắn vì không tốn hao phí chuyển đổi ngữ cảnh của hệ điều hành.

**Cài đặt Spinlock bằng lệnh Test-and-Set**:
```c
void acquire(lock) {
    while (test_and_set(&lock->flag));  // Vòng lặp xoay liên tục
}
void release(lock) {
    lock->flag = false;
}
```

| Tiêu chí so sánh | Khóa Mutex (Blocking) | Khóa xoay Spinlock |
| :--- | :--- | :--- |
| **Hành vi khi chờ khóa** | Luồng đi ngủ (blocked) | Luồng chạy vòng lặp kiểm tra liên tục (busy-wait) |
| **Sử dụng CPU khi chờ** | Gần như bằng 0 | Rất cao (lãng phí tài nguyên CPU) |
| **Hao phí chuyển ngữ cảnh** | Có (để đưa luồng đi ngủ và đánh thức lại) | Hoàn toàn không có |
| **Ngữ cảnh sử dụng tốt nhất** | Miền găng dài hoặc có liên quan đến chờ đợi I/O | Miền găng siêu ngắn, chỉ tốn vài chu kỳ CPU |

---

## Đèn hiệu (Semaphores)

Một **đèn hiệu (semaphore)** là một biến số nguyên dùng để đồng bộ hóa, ngoại trừ bước khởi tạo, nó chỉ có thể được truy cập thông qua hai thao tác nguyên tử: `wait()` (còn được gọi là phép toán $P$ hoặc giảm) và `signal()` (phép toán $V$ hoặc tăng).

### Đèn hiệu nhị phân (Binary Semaphore)
Chỉ nhận hai giá trị 0 và 1 – hoạt động hoàn toàn giống như một khóa Mutex.

```c
wait(S) {
    while (S <= 0);  // Chờ bận
    S--;
}
signal(S) {
    S++;
}
```

### Đèn hiệu đếm (Counting Semaphore)
Có thể nhận bất kỳ giá trị số nguyên không âm nào: đại diện cho số lượng tài nguyên dùng chung có sẵn trong hệ thống.

- Hàm `wait(S)` thực hiện giảm giá trị; nếu giá trị $S < 0$, tiến trình gọi sẽ bị chặn.
- Hàm `signal(S)` thực hiện tăng giá trị; nếu có tiến trình đang bị chặn, đánh thức một tiến trình hoạt động lại.

**Cài đặt Semaphore loại bỏ chờ bận (Blocking Semaphore)**:
```c
typedef struct {
    int value;
    struct process *waiting_queue;
} semaphore;

void wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        // Đưa tiến trình này vào hàng đợi chờ waiting_queue
        block();  // Hệ điều hành đưa tiến trình đi ngủ
    }
}

void signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {
        // Rút một tiến trình ra khỏi hàng đợi waiting_queue
        wakeup(); // Hệ điều hành đánh thức tiến trình dậy
    }
}
```

**Ví dụ thực tế**: Văn phòng có 3 chiếc máy in. Khởi tạo một đèn hiệu `printers = 3`. Mỗi tiến trình trước khi in phải gọi `wait(&printers)` để nhận máy, và gọi `signal(&printers)` sau khi in xong. Tiến trình thứ tư muốn in bắt buộc phải bị chặn ngủ đợi cho đến khi một trong ba tiến trình trước in xong và giải phóng máy.

---

## Các Bài toán Đồng bộ hóa Kinh điển

Các bài toán này là minh họa tiêu chuẩn cho các thách thức thiết kế đồng bộ hóa và cách sử dụng semaphore, mutex và monitor.

### 1. Bài toán Bộ đệm Giới hạn (Producer‑Consumer)

**Bài toán**: Có một bộ đệm chứa tối đa $N$ vị trí dữ liệu. Tiến trình sản xuất (*Producer*) ghi dữ liệu vào bộ đệm; tiến trình tiêu thụ (*Consumer*) lấy dữ liệu ra để xử lý. Yêu cầu: Tiến trình sản xuất không được phép ghi dữ liệu khi bộ đệm đã đầy; tiến trình tiêu thụ không được phép lấy dữ liệu khi bộ đệm rỗng. Đồng thời, tại một thời điểm chỉ cho phép tối đa một tiến trình được phép truy cập thay đổi bộ đệm.

**Giải pháp sử dụng Semaphore**:
```c
semaphore mutex = 1;     // Bảo vệ loại trừ tương hỗ khi truy cập bộ đệm
semaphore empty = n;     // Đếm số lượng ô nhớ trống còn lại trong bộ đệm
semaphore full = 0;      // Đếm số lượng ô nhớ đã chứa dữ liệu trong bộ đệm

// Tiến trình sản xuất (Producer)
do {
    // ... Tạo ra một phần tử dữ liệu mới ...
    wait(&empty);         // Chờ nếu không còn ô nhớ trống nào (empty = 0)
    wait(&mutex);         // Đi vào miền găng bảo vệ bộ đệm
    // ... Thêm dữ liệu vào bộ đệm ...
    signal(&mutex);       // Rời khỏi miền găng
    signal(&full);        // Tăng số lượng ô chứa dữ liệu lên 1
} while (true);

// Tiến trình tiêu thụ (Consumer)
do {
    wait(&full);          // Chờ nếu bộ đệm hoàn toàn rỗng (full = 0)
    wait(&mutex);         // Đi vào miền găng bảo vệ bộ đệm
    // ... Lấy dữ liệu ra khỏi bộ đệm ...
    signal(&mutex);       // Rời khỏi miền găng
    signal(&empty);       // Tăng số lượng ô nhớ trống lên 1
    // ... Xử lý phần tử dữ liệu ...
} while (true);
```

---

### 2. Bài toán Người đọc - Người viết (Readers‑Writers)

**Bài toán**: Một vùng dữ liệu (như cơ sở dữ liệu) được chia sẻ giữa nhiều tiến trình. Nhiều tiến trình đọc (*Readers*) được phép truy cập đọc dữ liệu đồng thời với nhau. Tuy nhiên, nếu một tiến trình viết (*Writer*) muốn thay đổi dữ liệu, nó yêu cầu quyền truy cập loại trừ tương hỗ tuyệt đối (không có reader hay writer nào khác được chạy chung).
- **Bài toán loại 1 (Ưu tiên Readers)**: Readers được ưu tiên truy cập; các Writers có thể gặp hiện tượng đói tài nguyên nếu luồng Readers liên tục gia nhập.
- **Bài toán loại 2 (Ưu tiên Writers)**: Writers được ưu tiên; Readers có thể bị đói tài nguyên nếu các yêu cầu ghi dữ liệu xuất hiện liên tục.

**Giải pháp cho Bài toán loại 1 (Ưu tiên Readers)**:
```c
semaphore rw_mutex = 1;   // Đồng bộ hóa quyền truy cập của Writer
semaphore mutex = 1;      // Bảo vệ biến đếm số lượng người đọc read_count
int read_count = 0;

// Tiến trình viết (Writer)
do {
    wait(&rw_mutex);
    // ... Thực hiện ghi dữ liệu vào cơ sở dữ liệu ...
    signal(&rw_mutex);
} while (true);

// Tiến trình đọc (Reader)
do {
    wait(&mutex);
    read_count++;
    if (read_count == 1)
        wait(&rw_mutex);   // Reader đầu tiên sẽ khóa không cho Writer truy cập
    signal(&mutex);
    
    // ... THỰC HIỆN ĐỌC DỮ LIỆU ...
    
    wait(&mutex);
    read_count--;
    if (read_count == 0)
        signal(&rw_mutex); // Reader cuối cùng rời đi sẽ mở khóa cho Writer
    signal(&mutex);
} while (true);
```

---

### 3. Bài toán Bữa ăn của các Triết gia (Dining Philosophers)

**Bài toán**: Có năm triết gia ngồi xung quanh một chiếc bàn tròn, trên bàn có năm chiếc đũa đặt xen kẽ giữa họ. Mỗi triết gia luân phiên giữa hai trạng thái là suy nghĩ và ăn uống. Để ăn, một triết gia bắt buộc phải cầm được hai chiếc đũa ở ngay bên trái và bên phải của mình. Thách thức là phải thiết kế thuật toán điều phối để không xảy ra bế tắc (*deadlock* - tất cả triết gia cùng đói và giữ một chiếc đũa mãi mãi) và đói tài nguyên (*starvation*).

**Giải pháp tránh bế tắc** (chỉ cho phép triết gia cầm đũa nếu cả hai chiếc đũa bên cạnh đều trống):
```c
semaphore chopstick[5] = {1,1,1,1,1};
semaphore mutex = 1;   // Khóa bảo vệ loại trừ tương hỗ khi kiểm tra đũa

void philosopher(int i) {
    while (true) {
        think();
        wait(&mutex);
        wait(&chopstick[i]);
        wait(&chopstick[(i+1)%5]);
        signal(&mutex);
        
        eat();
        
        signal(&chopstick[i]);
        signal(&chopstick[(i+1)%5]);
    }
}
```

---

### 4. Bài toán Người thợ cắt tóc đang ngủ (Sleeping Barber)

**Bài toán**: Một tiệm cắt tóc có một người thợ, một chiếc ghế cắt tóc và $N$ chiếc ghế trong phòng chờ. Nếu không có khách hàng, thợ cắt tóc sẽ ngồi ngủ trên ghế cắt. Khi một khách hàng đến: nếu thợ đang ngủ, khách hàng sẽ đánh thức thợ dậy để cắt tóc; nếu thợ đang bận cắt tóc cho người khác, khách hàng sẽ ngồi vào ghế chờ nếu phòng chờ còn ghế trống, ngược lại nếu phòng chờ đã hết ghế, khách hàng sẽ rời đi.

**Giải pháp sử dụng Semaphore**:
```c
semaphore customers = 0;   // Số lượng khách hàng đang chờ trong tiệm
semaphore barber = 0;      // Trạng thái thợ cắt tóc đã sẵn sàng
semaphore mutex = 1;       // Bảo vệ loại trừ tương hỗ khi thay đổi số ghế chờ
int waiting = 0;           // Số lượng khách đang ngồi trên ghế chờ
int CHAIRS = 5;            // Tổng số ghế chờ trong phòng

// Tiến trình Thợ cắt tóc (Barber)
do {
    wait(&customers);       // Ngủ nếu không có khách hàng nào
    wait(&mutex);
    waiting--;              // Một khách hàng rời ghế chờ để vào cắt tóc
    signal(&barber);        // Thợ sẵn sàng cắt tóc
    signal(&mutex);
    // ... Tiến hành cắt tóc cho khách ...
} while (true);

// Tiến trình Khách hàng (Customer)
wait(&mutex);
if (waiting < CHAIRS) {
    waiting++;
    signal(&customers);     // Đánh thức thợ cắt tóc nếu thợ đang ngủ
    signal(&mutex);
    wait(&barber);          // Chờ cho đến khi thợ sẵn sàng gọi mình vào
    // ... Được cắt tóc ...
} else {
    signal(&mutex);         // Tiệm đã hết ghế chờ, khách hàng rời đi
}
```

---

## Trình giám sát (Monitors) và Biến điều kiện

**Monitor** là một cấu trúc trừu tượng bậc cao cung cấp một cơ chế đồng bộ hóa tiến trình sạch sẽ và an toàn. Monitor được tích hợp trực tiếp dưới dạng một cú pháp của ngôn ngữ lập trình (ví dụ: trong Java, C#) hoặc thông qua thư viện hỗ trợ.

- Monitor **đóng gói** tất cả các biến chia sẻ và các phương thức truy cập vào trong một khối thống nhất.
- Tại một thời điểm, chỉ cho phép **tối đa một luồng** được phép hoạt động bên trong monitor (tự động thực thi loại trừ tương hỗ).
- **Biến điều kiện (Condition variables)** cho phép một luồng tạm thời đi ngủ chờ đợi bên trong monitor cho đến khi một điều kiện logic cụ thể được thỏa mãn, đồng thời tạm thời giải phóng quyền truy cập monitor cho luồng khác.

**Các thao tác trên biến điều kiện**:
- `wait(cond)`: Luồng gọi giải phóng quyền truy cập monitor và đi ngủ chờ trên biến điều kiện `cond`.
- `signal(cond)`: Đánh thức một luồng đang ngủ chờ trên biến điều kiện `cond` dậy để tiếp tục thực thi.

**Ví dụ: Bài toán Producer-Consumer sử dụng Monitor**:
```c
monitor BoundedBuffer {
    int buffer[N];
    int count = 0, in = 0, out = 0;
    condition notFull, notEmpty;

    void produce(int item) {
        while (count == N)
            wait(notFull);    // Chờ nếu bộ đệm đã đầy
        buffer[in] = item;
        in = (in+1) % N;
        count++;
        signal(notEmpty);     // Thông báo bộ đệm không còn rỗng
    }

    int consume() {
        while (count == 0)
            wait(notEmpty);   // Chờ nếu bộ đệm rỗng
        int item = buffer[out];
        out = (out+1) % N;
        count--;
        signal(notFull);      // Thông báo bộ đệm không còn đầy
        return item;
    }
}
```

---

## Đồng bộ hóa bằng thư viện POSIX Threads (Pthreads)

Thư viện POSIX threads (pthreads) cung cấp chuẩn giao tiếp API để thực hiện đồng bộ hóa trong ngôn ngữ C/C++ trên các hệ thống Unix/Linux.

### Khóa loại trừ tương hỗ Mutex
```c
#include <pthread.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

// Trong luồng thực thi
pthread_mutex_lock(&mutex);   // Nhận khóa
// --- Miền găng ---
pthread_mutex_unlock(&mutex); // Giải phóng khóa
```

### Biến điều kiện (Condition Variables)
Bắt buộc phải được sử dụng kết hợp với một khóa Mutex đi kèm:

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

// Luồng thực hiện chờ điều kiện thỏa mãn
pthread_mutex_lock(&mutex);
while (condition_not_met) {
    pthread_cond_wait(&cond, &mutex); // Tự động mở khóa mutex và đi ngủ
}
// Điều kiện đã thỏa mãn, tiến hành công việc
pthread_mutex_unlock(&mutex);

// Luồng thực hiện thay đổi điều kiện và đánh thức luồng khác
pthread_mutex_lock(&mutex);
// ... Thay đổi các biến điều kiện logic ...
pthread_cond_signal(&cond);   // Đánh thức một luồng đang ngủ
pthread_mutex_unlock(&mutex);
```

> [!WARNING]
> Luôn luôn sử dụng vòng lặp **`while`** (không dùng `if`) xung quanh lệnh `pthread_cond_wait()` để bảo vệ hệ thống tránh hiện tượng đánh thức giả (spurious wakeups) rất phổ biến trên các kiến trúc đa nhân.

---

## Bảng Tổng kết Chương

| Khái niệm | Điểm cốt lõi cần nhớ |
| :--- | :--- |
| **Miền găng** | Đoạn mã nguồn truy cập vào tài nguyên chia sẻ đòi hỏi bảo vệ loại trừ tương hỗ. |
| **Yêu cầu giải pháp** | Phải thỏa mãn đồng thời: loại trừ tương hỗ, tiến triển (progress), chờ đợi có giới hạn. |
| **Thuật toán Peterson** | Giải pháp phần mềm kinh điển cho hai tiến trình phục vụ mục đích giảng dạy. |
| **Hỗ trợ phần cứng** | Các lệnh nguyên tử TAS, CAS là nền tảng để xây dựng các công cụ khóa hiệu năng cao. |
| **Mutex vs. Spinlock** | Mutex đưa luồng đi ngủ (phù hợp miền găng dài); Spinlock chạy vòng lặp chờ bận (phù hợp miền găng siêu ngắn). |
| **Đèn hiệu (Semaphore)** | Biến đếm nguyên tử; Binary Semaphore tương tự Mutex, Counting Semaphore dùng để quản lý tài nguyên. |
| **Các bài toán kinh điển** | Bộ đệm giới hạn (Producer-Consumer), Readers-Writers, Dining Philosophers, Sleeping Barber. |
| **Monitor** | Cấu trúc trừu tượng cấp cao của ngôn ngữ lập trình đóng gói tự động loại trừ tương hỗ và biến điều kiện. |
| **Pthreads** | Chuẩn API lập trình đa luồng trên Unix với khóa Mutex và biến điều kiện để thiết lập đồng bộ hóa. |

Việc nắm vững các nguyên lý đồng bộ hóa tiến trình là kỹ năng tối quan trọng để viết các chương trình chạy song song và đa luồng chính xác. Chương tiếp theo sẽ giới thiệu về **Bế tắc (Deadlock)** – hiện tượng xảy ra khi quá trình thiết lập đồng bộ hóa gặp lỗi thiết kế.
