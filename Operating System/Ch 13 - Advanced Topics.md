# Chương 13: Các chủ đề nâng cao (Advanced Topics)

Chương này trình bày các lĩnh vực chuyên biệt của hệ điều hành, vượt ra ngoài phạm vi tính toán thông dụng: hệ thống nhúng và thời gian thực, nền tảng di động, phân tích hiệu năng và khả năng chịu lỗi. Các chủ đề này cho thấy các nguyên lý của hệ điều hành được điều chỉnh như thế nào để thích ứng với các ràng buộc và môi trường khác nhau.

---

## Hệ điều hành thời gian thực (Real‑Time Operating Systems - RTOS)

Một **Hệ điều hành thời gian thực** (Real-Time Operating System - RTOS) đảm bảo rằng các tác vụ phải hoàn thành trong một thời hạn định (timing deadline) cụ thể. Tính đúng đắn của hệ thống không chỉ phụ thuộc vào kết quả logic mà còn phụ thuộc vào thời điểm kết quả đó được đưa ra.

### So sánh Thời gian thực Cứng và Mềm (Hard vs Soft Real‑Time)

| Phân loại | Hậu quả khi trễ hạn định (Deadline violation) | Ví dụ |
|------|-------------------------------|----------|
| **Thời gian thực cứng** (Hard real‑time) | Thảm khốc (gây lỗi hệ thống nghiêm trọng, chấn thương người dùng) | Bung túi khí ô tô, máy trợ tim, điều khiển bay |
| **Thời gian thực mềm** (Soft real‑time) | Giảm hiệu năng (giật lag đa phương tiện, giảm trải nghiệm) | Phát video, truyền phát âm thanh, trò chơi điện tử |

### Các đặc điểm chính của RTOS

- **Lập lịch tiền định (Deterministic scheduling)** (lập lịch ưu tiên tiền định, ví dụ như thuật toán Rate‑Monotonic).
- **Độ trễ ngắt thấp (Low interrupt latency)** (khoảng thời gian cực ngắn từ khi xuất hiện ngắt đến khi bắt đầu thực thi trình xử lý ngắt).
- **Thời gian bị chặn tối thiểu (Minimal blocking)** (sử dụng cơ chế kế thừa độ ưu tiên - priority inheritance nhằm ngăn chặn hiện tượng nghịch đảo độ ưu tiên - priority inversion).
- **Cấp phát bộ nhớ có thể dự đoán (Predictable memory allocation)** (tránh cấp phát bộ nhớ động thông thường hoặc cung cấp các trình cấp phát tiền định sẵn).

### Ví dụ điển hình: VxWorks và FreeRTOS

| Đặc điểm (Feature) | **VxWorks** (Wind River) | **FreeRTOS** (Amazon) |
|---------|--------------------------|------------------------|
| Phân loại | Thương mại, thời gian thực cứng | Mã nguồn mở, siêu nhẹ |
| Kiến trúc nhân | Nhân liền khối (Monolithic) có các phần mở rộng dạng mô-đun | Dạng vi nhân (Microkernel) (nhân tối thiểu) |
| Phần cứng điển hình | PowerPC, ARM, x86 (phân khúc cao cấp) | ARM Cortex‑M, AVR, các bộ vi điều khiển (MCU) nhỏ |
| Dung lượng bộ nhớ (Memory footprint) | Hàng trăm KB đến hàng MB | Cực nhỏ chỉ khoảng 4 - 10 KB |
| Lập lịch (Scheduling) | Tiền định dựa trên độ ưu tiên, xoay vòng (round-robin), phân chia thời gian | Tiền định dựa trên độ ưu tiên, có tùy chọn hợp tác (cooperative) |
| Ứng dụng thực tế | Robot tự hành sao Hỏa, hàng không vũ trụ, robot công nghiệp | Cảm biến IoT, thiết bị điện tử gia dụng, dự án cá nhân/học tập |

**So sánh thực tế**: 
- **Thời gian thực cứng**: Hệ thống túi khí của ô tô – bắt buộc phải bung ra trong vòng vài mili giây kể từ khi xảy ra va chạm; chỉ cần trễ $1\text{ ms}$ có thể đe dọa đến tính mạng.
- **Thời gian thực mềm**: Chương trình nghe nhạc – độ trễ $50\text{ ms}$ trong việc giải mã âm thanh chỉ gây ra tiếng giật nhẹ khó chịu chứ không gây thảm họa.

---

## Hệ điều hành nhúng (Embedded Operating Systems)

Các hệ điều hành nhúng chạy trên các thiết bị chuyên dụng bị giới hạn nghiêm ngặt về tài nguyên (CPU, bộ nhớ, điện năng). Chúng được tối ưu hóa đặc biệt về **hiệu năng (efficiency)**, **độ tin cậy (reliability)** và hành vi **thời gian thực (real-time)**.

### Các đặc trưng của hệ điều hành nhúng

- **Giới hạn tài nguyên**: Bộ nhớ RAM chỉ từ vài KB đến hàng MB, bộ vi xử lý tốc độ MHz.
- **Đơn mục tiêu**: Thường chỉ chạy một ứng dụng duy nhất mãi mãi (ví dụ: lò vi sóng, máy điều hòa nhiệt độ).
- **Cấu hình tĩnh (Static configuration)**: Không có cài đặt phần mềm từ người dùng hoặc tải ứng dụng động.
- **Thời gian khởi động cực nhanh**: Thường tính bằng giây hoặc tức thì.

### Các hệ điều hành nhúng phổ biến

| Hệ điều hành | Ứng dụng điển hình |
|----|--------------|
| **FreeRTOS** | Bộ vi điều khiển, cảm biến IoT |
| **Zephyr** | Các thiết bị đeo thông minh (wearables), phần cứng nhúng (Linux Foundation) |
| **Linux nhúng** (Embedded Linux) | Bộ định tuyến (routers), đầu thu truyền hình (set-top boxes), hệ thống giải trí xe hơi (có khả năng tùy biến cao nhưng nặng hơn) |
| **RT‑Linux (PREEMPT_RT)** | Nhân Linux được bổ sung các bản vá thời gian thực dùng trong điều khiển công nghiệp |
| **ThreadX** (Azure RTOS) | Các ứng dụng yêu cầu độ tin cậy tối đa (vũ trụ, thiết bị y sinh) |

### So sánh: Linux nhúng với RTOS

- **Linux nhúng** (Yocto, Buildroot): Cung cấp đầy đủ hệ điều hành, ngăn xếp mạng, trình điều khiển thiết bị phong phú nhưng dung lượng lớn (nhân tối thiểu $1 - 10\text{ MB}$). Không có tính tiền định ngoại trừ khi được vá.
- **FreeRTOS**: Nhân siêu nhỏ ($4 - 10\text{ KB}$), có tính tiền định thời gian thực cao nhưng có ít tính năng tích hợp sẵn.

**So sánh thực tế**: 
- **Linux nhúng**: Giống như một máy tính mini tích hợp trong màn hình điều khiển ô tô – mạnh mẽ, nhiều tính năng nhưng cần thời gian để khởi động.
- **FreeRTOS**: Giống như bộ vi điều khiển trong đồng hồ kỹ thuật số – siêu nhỏ, bật lên là chạy ngay lập tức nhưng chỉ làm tốt một nhiệm vụ duy nhất.

---

## Các tính năng của Hệ điều hành di động (Mobile Operating System Features)

Hệ điều hành di động (Android, iOS) kế thừa các tính năng hệ điều hành thông dụng nhưng được thiết kế riêng cho các ràng buộc đặc thù: **thời lượng pin**, **tích hợp cảm biến** và **tính cơ động của người dùng**.

### Quản lý điện năng (Power Management)

Dung lượng pin luôn bị giới hạn. Do đó, hệ điều hành di động áp dụng các cơ chế tiết kiệm điện năng cực kỳ triệt để:

| Kỹ thuật | Mô tả |
|-----------|-------------|
| **CPU throttling** (Điều tiết CPU) | Tự động giảm tần số (xung nhịp) hoặc tắt bớt nhân CPU khi hệ thống rảnh rỗi (cơ chế DVFS – Dynamic Voltage & Frequency Scaling). |
| **Suspend states** (Trạng thái tạm ngưng) | Chế độ ngủ sâu (Doze mode trên Android, App Nap trên iOS). CPU sẽ ngừng chạy hoàn toàn và chỉ thức dậy thông qua các sự kiện ngắt. |
| **Background restrictions** (Hạn chế chạy nền) | Giới hạn băng thông mạng và chu kỳ CPU của các ứng dụng chạy nền (Android Background Execution Limits, iOS Background App Refresh). |
| **Battery‑aware scheduling** (Lập lịch tiết kiệm pin) | Cơ chế EAS (Energy-Aware Scheduling) trên Linux/Android: tự động phân bổ và chạy các tác vụ trên các nhân CPU tiết kiệm điện nhất. |

### Tích hợp hệ thống cảm biến (Sensor Integration)

Hệ điều hành di động cung cấp các khung phần mềm (framework) để trừu tượng hóa các cảm biến vật lý trên thiết bị:

| Cảm biến | Ứng dụng phổ biến | Trừu tượng hóa của HĐH |
|--------|-------------|----------------|
| **Accelerometer** (Cảm biến gia tốc) | Tự động xoay màn hình, đếm bước chân | SensorManager (Android), CoreMotion (iOS) |
| **Gyroscope** (Cảm biến con quay hồi chuyển) | Điều khiển hướng trong game, thực tế ảo (AR) | Tương tự cảm biến gia tốc |
| **GPS** (Hệ thống định vị toàn cầu) | Dịch vụ bản đồ, định vị vị trí | LocationManager |
| **Camera** (Máy ảnh) | Chụp ảnh, quét mã vạch/QR | Camera API |
| **Fingerprint** (Cảm biến vân tay) | Xác thực sinh trắc học, mở khóa thiết bị | BiometricPrompt |

### Các tính năng đặc thù khác trên thiết bị di động

- **Cơ chế hộp cát ứng dụng (App sandboxing)**: Mỗi ứng dụng được chạy dưới dạng một tài khoản người dùng độc lập (UID) với các quyền hạn bị giới hạn nghiêm ngặt (cơ chế cô lập UID trên Android, App Sandbox trên iOS).
- **Thông báo đẩy (Push notifications)**: Đánh thức ứng dụng để nhận dữ liệu mà không cần duy trì ứng dụng đó chạy ngầm liên tục (Google FCM, Apple APNs).
- **Cập nhật hệ thống liền mạch (Seamless updates)**: Sử dụng hai phân vùng hệ thống song song (phân vùng A/B) – tiến hành cập nhật hệ thống ở phân vùng ẩn khi máy đang chạy, sau đó chỉ cần khởi động lại để chuyển sang phân vùng mới.

**So sánh thực tế**: Chiếc điện thoại thông minh giống như một con dao đa năng Thụy Sĩ – chứa rất nhiều công cụ hữu ích trong một thân máy nhỏ, nhưng dung lượng pin có hạn. Cơ chế quản lý điện năng giống như việc con dao này tự động gập các lưỡi dao lại khi bạn không dùng đến để bảo vệ lưỡi và tiết kiệm năng lượng.

---

## Phân tích hiệu năng hệ thống (System Performance Profiling - perf, DTrace)

Các công cụ phân tích hiệu năng (profiling tools) giúp lập trình viên và quản trị viên hệ thống hiểu rõ hệ thống đang tiêu tốn thời gian và tài nguyên CPU ở những vùng mã nguồn nào.

### Công cụ perf (trên Linux)

`perf` là công cụ phân tích hiệu năng tiêu chuẩn trên hệ điều hành Linux. Nó tận dụng các bộ đếm hiệu năng phần cứng (hardware performance counters) để đo lường các chu kỳ CPU (CPU cycles), lỗi trượt bộ nhớ đệm (cache misses), đoán sai nhánh rẽ (branch mispredictions) cùng các sự kiện phần mềm như chuyển đổi ngữ cảnh (context switches) và lỗi trang (page faults).

**Các câu lệnh thông dụng**:

```bash
perf stat ls          # Đếm các sự kiện phần cứng khi thực thi một câu lệnh
perf record -g ls     # Lấy mẫu dấu vết ngăn xếp (call stack graph)
perf report           # Xem và phân tích kết quả đã ghi lại
perf top              # Xem trực tiếp các hàm đang chiếm nhiều CPU nhất
```

### Công cụ DTrace (trên Solaris, BSD, macOS)

Được phát triển bởi Sun Microsystems, DTrace cho phép **bố trí công cụ động (dynamic instrumentation)** – bạn có thể theo vết các hàm trong không gian nhân (kernel) hoặc không gian người dùng (user space) mà không cần phải biên dịch lại mã nguồn hay khởi động lại hệ thống. DTrace sử dụng ngôn ngữ kịch bản D.

**Các trường hợp sử dụng điển hình**:
- Theo vết lời gọi hệ thống: `dtrace -n 'syscall:::entry { printf("%s\n", probefunc); }'`
- Phân tích độ trễ của các thao tác I/O trên tệp tin.
- Theo dõi các hoạt động cấp phát và giải phóng bộ nhớ.

### Các công cụ phân tích hiệu năng khác

| Công cụ | Nền tảng | Mục đích sử dụng |
|------|----------|---------|
| **strace** | Linux | Theo vết các lời gọi hệ thống (system calls) |
| **ltrace** | Linux | Theo vết các lời gọi thư viện chia sẻ (shared library calls) |
| **Valgrind** | Linux | Phân tích rò rỉ bộ nhớ và giả lập hành vi bộ đệm |
| **eBPF/bpftrace** | Linux | Công cụ theo vết động hiện đại (tập siêu mở rộng của các khái niệm DTrace) |
| **Instruments** | macOS/iOS | Công cụ phân tích hiệu năng giao diện đồ họa trực quan (CPU, bộ nhớ, năng lượng) |
| **Android Studio Profiler** | Android | Công cụ phân tích CPU, bộ nhớ và lưu lượng mạng của ứng dụng |

**So sánh thực tế**: 
- **perf**: Giống như đồng hồ đo mức tiêu thụ nhiên liệu trên xe ô tô – cho bạn biết chính xác lượng xăng (chu kỳ CPU) bị tiêu hao ở mỗi chặng đường của hành trình.
- **DTrace**: Giống như một bảng điều khiển di động với các cảm biến mà bạn có thể gắn nhanh vào bất kỳ bộ phận nào của xe ngay khi xe đang chạy mà không cần phải dừng xe (dynamic instrumentation).

---

## Khả năng chịu lỗi và Tạo điểm kiểm tra (Fault Tolerance and Checkpointing)

**Khả năng chịu lỗi (Fault tolerance)** là khả năng hệ thống tiếp tục hoạt động chính xác ngay cả khi xảy ra các sự cố lỗi (về phần cứng, phần mềm hoặc kết nối mạng). **Tạo điểm kiểm tra (Checkpointing)** là một kỹ thuật cốt lõi để hiện thực hóa khả năng này.

### Tạo điểm kiểm tra (Checkpointing)

Định kỳ lưu lại toàn bộ trạng thái hiện tại của một tiến trình (nội dung bộ nhớ, các thanh ghi, các tệp đang mở) vào một vùng lưu trữ bền vững. Nếu tiến trình bị sập (crash), nó có thể **khởi động lại từ điểm kiểm tra gần nhất** (cơ chế phục hồi quay lui - rollback recovery).

**Các phân loại điểm kiểm tra**:

| Phân loại | Mô tả | Chi phí phụ trội (Overhead) |
|------|-------------|----------|
| **Cấp ứng dụng** (Application‑level) | Lập trình viên tự viết các lệnh lưu trạng thái trực tiếp trong mã nguồn. | Thấp (lưu thông tin có chọn lọc) |
| **Cấp hệ thống** (System‑level) | Hệ điều hành tự động lưu toàn bộ trạng thái tiến trình một cách trong suốt (ví dụ: CRIU trên Linux). | Cao (phải ghi lại toàn bộ bản đồ bộ nhớ) |
| **Gắn thêm/Lũy tiến** (Incremental) | Chỉ lưu lại các trang bộ nhớ có thay đổi (dirty pages) kể từ lần tạo điểm kiểm tra trước đó. | Trung bình |

### CRIU (Checkpoint/Restore In Userspace)

Một công cụ mạnh mẽ trên Linux có khả năng "đóng băng" một container hoặc một tiến trình đang chạy, lưu toàn bộ trạng thái của nó xuống đĩa và sau đó khôi phục lại trạng thái hoạt động chính xác đó – thậm chí có thể khôi phục trên một máy tính vật lý hoàn toàn khác.

**Ứng dụng**: Di trú trực tiếp các container (live migration) giữa các máy chủ cloud, tạo nhanh các bản sao hệ thống (snapshots) để phục vụ gỡ lỗi.

### Các kỹ thuật chịu lỗi khác

| Kỹ thuật | Mô tả |
|-----------|-------------|
| **Sao chép** (Replication) | Chạy song song nhiều bản sao giống hệt nhau; tiến hành bỏ phiếu (vote) trên các kết quả đầu ra để chọn ra kết quả đúng. |
| **Chính/Phụ** (Primary/backup) | Một nút đóng vai trò chính (active); nút dự phòng (standby) liên tục lắng nghe và lập tức tiếp quản hệ thống nếu nút chính gặp sự cố (thông qua tín hiệu nhịp tim - heartbeats). |
| **Giao dịch (ACID)** | Đảm bảo tính nguyên tử (atomic), nhất quán, cô lập và bền vững của các thao tác dữ liệu – tự động quay lui (rollback) nếu có bước lỗi xảy ra. |
| **RAID (đĩa cứng)** | Mảng ổ đĩa dự phòng giúp bảo vệ dữ liệu và duy trì hoạt động của hệ thống khi ổ cứng bị hỏng (đã học ở Chương 9). |
| **Bộ định thời Watchdog** (Watchdog timers) | Định kỳ đếm ngược và tự động khởi động lại (reset) hệ thống nếu tiến trình phần mềm bị treo và không kịp gửi tín hiệu đặt lại bộ định thời (rất phổ biến trong hệ thống nhúng/RTOS). |

### Các mô hình lỗi trong hệ thống phân tán (Failure Models)

Trong hệ thống phân tán, các sự cố lỗi được chia thành các nhóm:

- **Lỗi dừng sạch (Fail‑stop)**: Một nút dừng hoạt động hoàn toàn và các nút khác trong hệ thống có thể phát hiện ra điều đó một cách rõ ràng.
- **Lỗi Byzantine (Byzantine failure)**: Một nút hoạt động sai lệch một cách ngẫu nhiên hoặc cố ý (do lỗi phần mềm nghiêm trọng hoặc bị tấn công độc hại). Việc giải quyết loại lỗi này yêu cầu các thuật toán chịu lỗi Byzantine (BFT) cực kỳ phức tạp và tốn kém chi phí.
- **Phân mảnh mạng (Network partition)**: Một nhóm các nút không thể kết nối hoặc giao tiếp với nhóm các nút còn lại do sự cố đường truyền mạng.

**So sánh thực tế**: Checkpointing tương tự như tính năng lưu trò chơi (save game) tự động sau mỗi vài phút trong các trò chơi điện tử. Nếu trò chơi bị sập hoặc nhân vật của bạn bị thua, bạn chỉ cần tải lại (load) dữ liệu từ lần lưu gần nhất thay vì phải chơi lại từ đầu.

---

## Tóm tắt (Summary)

| Chủ đề | Điểm mấu chốt (Key takeaway) |
|-------|--------------|
| **RTOS** | Đảm bảo tính chính xác về mặt thời gian và hạn định (deadline); VxWorks (thời gian thực cứng, thương mại), FreeRTOS (siêu nhẹ, mã nguồn mở). |
| **HĐH Nhúng** | Được thiết kế tối giản cho phần cứng bị giới hạn tài nguyên và chạy một mục đích chuyên biệt; phổ biến là FreeRTOS và Linux nhúng. |
| **HĐH Di động** | Tập trung vào quản lý điện năng triệt để (CPU throttling, chế độ ngủ sâu), giao tiếp cảm biến và bảo mật bằng cơ chế hộp cát (sandboxing). |
| **Phân tích hiệu năng** | Sử dụng công cụ `perf` (dựa trên bộ đếm phần cứng Linux) và `DTrace` (theo vết động mã nguồn thông qua dynamic instrumentation). |
| **Checkpointing** | Định kỳ sao lưu trạng thái tiến trình; công cụ CRIU cho phép di trú trực tiếp các tiến trình/container giữa các máy chủ vật lý. |
| **Khả năng chịu lỗi** | Được xây dựng thông qua cơ chế sao chép (replication), cấu hình chính/phụ (primary/backup), giao dịch ACID, cấu hình RAID và bộ định thời watchdog. |

Các chủ đề nâng cao trên đây chứng minh sự đa dạng của hệ điều hành ngoài phạm vi máy tính để bàn thông thường. Các ràng buộc thời gian thực, giới hạn điện năng tiêu thụ, tối ưu hóa hiệu năng và khả năng chống chịu sự cố là các yếu tố vô cùng sống còn trong thế giới tính toán hiện đại – từ các cảm biến IoT siêu nhỏ cho đến các trung tâm dữ liệu đám mây khổng lồ.
