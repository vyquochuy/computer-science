## **Quản lý Luồng (Thread Management)**

### **1. Giới thiệu**

Quản lý luồng (*Thread Management*) là một nhiệm vụ cốt lõi của Hệ điều hành (OS) chịu trách nhiệm cho việc tạo lập, lập lịch, thực thi, đồng bộ hóa và kết thúc các **luồng (threads)**.
Một **luồng** là đơn vị thực thi CPU nhỏ nhất bên trong một tiến trình. Nhiều luồng khác nhau trong cùng một tiến trình chia sẻ chung các tài nguyên hệ thống như bộ nhớ, tệp và mã nguồn, giúp việc thực thi đa luồng có hiệu năng và tốc độ vượt trội hơn rất nhiều so với mô hình đa tiến trình trong hầu hết các ngữ cảnh thực tế.

---

### **2. Cấu trúc của Luồng**

Một **luồng** (còn được gọi là tiến trình siêu nhẹ - lightweight process) bao gồm các phần riêng biệt:

* Mã định danh luồng (Thread ID - TID)
* Con trỏ chương trình (Program Counter)
* Tập thanh ghi CPU (Register Set)
* Vùng nhớ ngăn xếp riêng (Stack)

Các luồng trong cùng một tiến trình sẽ **chia sẻ chung**:

* Phân đoạn mã lệnh (Code section)
* Phân đoạn dữ liệu tĩnh (Data section)
* Vùng nhớ Heap
* Các tệp tin đang mở và tài nguyên vào/ra (I/O)

Mô hình chia sẻ tài nguyên này cho phép giao tiếp cực kỳ nhanh chóng và giảm thiểu tối đa hao phí hệ thống.

---

### **3. Tiến trình Đơn luồng vs. Đa luồng**

| Đặc điểm so sánh | Đơn luồng (Single-Threaded) | Đa luồng (Multithreaded) |
| :--- | :--- | :--- |
| **Luồng điều khiển CPU** | Duy nhất một luồng | Nhiều luồng đồng thời |
| **Hiệu suất sử dụng CPU** | Thấp | Cao |
| **Tốc độ phản hồi** | Kém | Tốt hơn nhiều |
| **Chi phí chuyển đổi ngữ cảnh** | Cao | Thấp |
| **Chia sẻ tài nguyên** | Không áp dụng | Cực kỳ hiệu quả |

---

### **4. Lợi ích của Đa luồng**

* **Tốc độ phản hồi (Responsiveness)**: Một luồng có thể tiếp tục chạy bình thường ngay cả khi luồng khác cùng tiến trình đang bị chặn (block) để đợi I/O.
* **Chia sẻ tài nguyên (Resource Sharing)**: Các luồng chia sẻ chung bộ nhớ và tệp tin có sẵn của tiến trình cha một cách tự nhiên.
* **Tính kinh tế (Economy)**: Việc tạo lập và chuyển đổi luồng rẻ hơn rất nhiều so với tiến trình.
* **Khả năng mở rộng (Scalability)**: Tận dụng cực kỳ hiệu quả kiến trúc bộ vi xử lý đa nhân.

---

### **5. Các Trạng thái của Luồng (Thread States)**

Các trạng thái phổ biến của luồng bao gồm:

* **Mới (New)**
* **Sẵn sàng (Ready)**
* **Đang chạy (Running)**
* **Bị chặn / Đang chờ (Blocked / Waiting)**
* **Kết thúc (Terminated)**

---

### **6. Khối Quản lý Luồng (Thread Control Block - TCB)**

Mỗi luồng được đại diện trong hệ thống bởi một **Khối Quản lý Luồng (Thread Control Block - TCB)** để lưu trữ các thông tin:

* Mã định danh luồng (Thread ID)
* Trạng thái hiện tại của luồng
* Con trỏ chương trình (Program counter)
* Giá trị các thanh ghi CPU
* Con trỏ ngăn xếp (Stack pointer)
* Thông tin lập lịch độ ưu tiên

Hệ điều hành sử dụng TCB để quản lý việc thực thi luồng và thực hiện chuyển đổi ngữ cảnh giữa các luồng.

---

### **7. Lập lịch Luồng (Thread Scheduling)**

Lập lịch luồng quyết định luồng nào sẽ được cấp phát thời gian sử dụng CPU.

#### **Các loại lập lịch luồng**

* **Lập lịch Trưng dụng (Preemptive Scheduling)**: Hệ điều hành có quyền ngắt ngang luồng đang chạy.
* **Lập lịch Không trưng dụng (Non-Preemptive Scheduling)**: Luồng tự chạy cho đến khi bị chặn I/O hoặc tự nguyện kết thúc.

Các luồng thường được lập lịch thông qua:
* Thuật toán xoay vòng Round Robin
* Lập lịch dựa trên độ ưu tiên (Priority-based scheduling)
* Hàng đợi nhiều cấp (Multilevel queue scheduling)

---

### **8. Chuyển đổi Ngữ cảnh giữa các Luồng**

Chuyển đổi ngữ cảnh giữa các luồng trong **cùng một tiến trình** nhanh hơn nhiều so với giữa các tiến trình vì:

* Không gian địa chỉ bộ nhớ được giữ nguyên hoàn toàn.
* Không cần phải nạp lại hoặc xóa bộ đệm bảng trang (page tables / TLB).

Điều này mang lại sự cải thiện hiệu năng vượt trội cho các ứng dụng đa luồng.

---

### **9. Luồng cấp Người dùng vs. Luồng cấp Nhân**

| Tiêu chí so sánh | Luồng cấp Người dùng | Luồng cấp Nhân |
| :--- | :--- | :--- |
| **Quản lý bởi** | Thư viện không gian người dùng | Nhân hệ điều hành |
| **Chi phí chuyển ngữ cảnh** | Cực kỳ thấp | Cao hơn |
| **Khi gọi hàm chặn I/O** | Chặn toàn bộ tiến trình cha | Chỉ chặn duy nhất luồng con đó |
| **Khả năng chạy song song** | Không song song thực tế | Chạy song song thực sự trên đa nhân |

---

### **10. Các Mô hình Đa luồng**

* **Nhiều-Một (Many-to-One)**: Nhiều luồng người dùng ánh xạ vào một luồng nhân duy nhất.
* **Một-Một (One-to-One)**: Mỗi luồng người dùng ánh xạ trực tiếp tới một luồng nhân riêng lẻ.
* **Nhiều-Nhiều (Many-to-Many)**: Nhiều luồng người dùng ánh xạ dồn dịch vào nhiều luồng nhân.

---

### **11. Đồng bộ hóa Luồng (Thread Synchronization)**

Do các luồng dùng chung vùng nhớ bộ nhớ RAM, việc đồng bộ hóa là cực kỳ quan trọng để tránh hiện tượng tranh chấp dữ liệu (race conditions).

Các cơ chế đồng bộ hóa phổ biến:

* Khóa loại trừ tương hỗ (Mutex Locks)
* Đèn hiệu (Semaphores)
* Biến điều kiện (Condition Variables)
* Khóa xoay (Spinlocks)
* Trình giám sát (Monitors)

Các cơ chế này đảm bảo **loại trừ tương hỗ** (mutual exclusion) và **tính nhất quán của dữ liệu** (data consistency).

---

### **12. Giao tiếp giữa các Luồng (Thread Communication)**

Các luồng trao đổi thông tin thông qua:

* Các biến chia sẻ (Shared variables)
* Các cấu trúc dữ liệu dùng chung (Shared data structures)
* Hàng đợi an toàn đa luồng (Thread-safe queues)

Khác với tiến trình, luồng không đòi hỏi các cơ chế giao tiếp liên tiến trình IPC (như socket hoặc pipe) nặng nề.

---

### **13. Tạo lập và Kết thúc Luồng**

Luồng được tạo thông qua các lời gọi thư viện hoặc hệ thống như:

* Hàm `pthread_create()` (Thư viện POSIX Threads)
* Lớp `std::thread` (Ngôn ngữ C++)
* Lớp `Thread` (Ngôn ngữ Java)

Luồng kết thúc khi:

* Luồng hoàn thành toàn bộ khối lệnh thực thi.
* Luồng bị hủy bỏ (cancelled) một cách chủ động.
* Tiến trình cha chứa nó bị kết thúc (tất cả các luồng con sẽ tự động dừng).

---

### **14. Thách thức trong Quản lý Luồng**

* Tranh chấp dữ liệu (Race conditions)
* Bế tắc (Deadlocks)
* Đói tài nguyên (Starvation)
* Nghịch đảo độ ưu tiên (Priority inversion)
* Độ phức tạp cực cao khi gỡ lỗi (Debugging complexity)

Cần áp dụng các chính sách đồng bộ hóa và lập lịch đúng đắn để giải quyết triệt để các vấn đề này.

---

### **15. Tầm quan trọng của Quản lý Luồng**

Quản lý luồng là vô cùng quan trọng bởi vì nó:

* Nâng cao hiệu năng tổng thể của ứng dụng.
* Cho phép xử lý song song thực sự trên hệ thống đa nhân.
* Tăng tốc độ phản hồi của hệ thống với người dùng.
* Tiết kiệm tài nguyên và hao phí hơn nhiều so với mô hình đa tiến trình.

---

### **16. Kết luận**

Quản lý luồng cho phép hệ điều hành thực thi đồng thời nhiều tác vụ một cách cực kỳ hiệu quả bên trong một tiến trình duy nhất. Bằng cách quản lý chặt chẽ việc tạo, lập lịch, đồng bộ hóa và kết thúc luồng, hệ điều hành đảm bảo hiệu năng cao, khả năng mở rộng tốt và sự thực thi tin cậy của các hệ thống máy tính hiện đại.
