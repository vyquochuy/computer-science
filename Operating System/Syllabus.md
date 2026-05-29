# HỆ ĐIỀU HÀNH (OPERATING SYSTEMS)

## Chương 1: Giới thiệu về Hệ điều hành

* Hệ điều hành là gì?
* Các vai trò của một Hệ điều hành: quản lý tài nguyên (resource manager), máy ảo (virtual machine), giao diện giao tiếp (interface)
* Kiến trúc hệ thống máy tính: CPU, bộ nhớ, thiết bị vào/ra (I/O)
* Cấu trúc Hệ điều hành: khối đơn nhất (monolithic), phân tầng (layered), vi nhân (microkernel), lai (hybrid)
* Chế độ người dùng (User mode) so với Chế độ đặc quyền (Kernel mode)
* Lời gọi hệ thống (System calls) và API
* Các dịch vụ và tiện ích của Hệ điều hành
* Quy trình khởi động máy tính (Booting process)

---

## Chương 2: Quản lý Tiến trình (Process Management)

* Khái niệm Tiến trình (*Process*): chương trình (program) so với tiến trình
* Các trạng thái của Tiến trình: mới (new), sẵn sàng (ready), đang chạy (running), đang chờ (waiting), kết thúc (terminated)
* Khối quản lý tiến trình (Process Control Block - PCB)
* Các hàng đợi lập lịch tiến trình (Scheduling queues)
* Chuyển đổi ngữ cảnh (Context switching)
* Tạo lập và kết thúc tiến trình (các hàm fork, exec, wait, exit)
* Hệ phân cấp tiến trình (Process hierarchies)
* Tiến trình so với Luồng (*Thread*)
* Luồng cấp người dùng (User-level threads) so với Luồng cấp nhân (Kernel-level threads)
* Các mô hình đa luồng (Multithreading models): nhiều-một (many-to-one), một-một (one-to-one), nhiều-nhiều (many-to-many)

---

## Chương 3: Lập lịch CPU (CPU Scheduling)

* Các tiêu chí lập lịch: hiệu suất sử dụng CPU, thông lượng (throughput), thời gian hoàn thành (turnaround time), thời gian chờ (waiting time), thời gian phản hồi (response time)
* Lập lịch trưng dụng (Preemptive) so với Không trưng dụng (Non-preemptive)
* Các giải thuật lập lịch:
  * Đến trước, phục vụ trước (First-Come, First-Served - FCFS)
  * Công việc ngắn nhất trước (Shortest Job First - SJF) – không trưng dụng và trưng dụng (SRTF)
  * Xoay vòng (Round Robin - RR)
  * Lập lịch theo độ ưu tiên (Priority scheduling - kết hợp cơ chế tăng tuổi thọ/aging)
  * Lập lịch hàng đợi nhiều cấp (Multilevel queue scheduling)
  * Lập lịch hàng đợi phản hồi nhiều cấp (Multilevel feedback queue)
* Lập lịch luồng (Thread scheduling)
* Lập lịch thời gian thực (rate-monotonic, earliest deadline first)

---

## Chương 4: Đồng bộ hóa Tiến trình (Process Synchronization)

* Vấn đề miền găng (Critical section problem)
* Các yêu cầu giải pháp: loại trừ tương hỗ (mutual exclusion), tiến triển (progress), chờ đợi có giới hạn (bounded waiting)
* Các giải pháp phần mềm: Thuật toán Peterson
* Hỗ trợ phần cứng: test-and-set, compare-and-swap
* Khóa loại trừ tương hỗ (Mutex locks) và khóa xoay (spinlocks)
* Đèn hiệu (Semaphores) (binary và counting semaphores)
* Các bài toán đồng bộ hóa kinh điển:
  * Bộ đệm giới hạn (Bounded buffer - Producer/Consumer)
  * Người đọc - Người viết (Readers-Writers)
  * Bữa ăn của các triết gia (Dining philosophers)
  * Người thợ cắt tóc đang ngủ (Sleeping barber)
* Trình giám sát (Monitors) và biến điều kiện (condition variables)
* Đồng bộ hóa trong POSIX threads (mutex, biến điều kiện)

---

## Chương 5: Bế tắc (Deadlocks)

* Các đặc trưng của Bế tắc: loại trừ tương hỗ, giữ và chờ (hold and wait), không trưng dụng (no preemption), chờ đợi vòng tròn (circular wait)
* Đồ thị cấp phát tài nguyên (Resource allocation graph)
* Các phương pháp xử lý bế tắc:
  * Ngăn ngừa bế tắc (Deadlock prevention - phá vỡ một trong bốn điều kiện)
  * Tránh bế tắc (Deadlock avoidance - Thuật toán Banker)
  * Phát hiện bế tắc (Deadlock detection - đi kèm phục hồi)
  * Bỏ qua bế tắc (Deadlock ignorance - Thuật toán đà điểu/Ostrich)

---

## Chương 6: Quản lý Bộ nhớ (Memory Management)

* Phân cấp bộ nhớ (Memory hierarchy)
* Liên kết địa chỉ (Address binding): lúc biên dịch (compile time), lúc nạp (load time), lúc thực thi (execution time)
* Không gian địa chỉ logic so với vật lý
* Cấp phát bộ nhớ:
  * Cấp phát liên tục (Contiguous allocation - phân hoạch cố định và động)
  * Phân mảnh ngoại vi (External fragmentation) và phân mảnh nội vi (Internal fragmentation)
  * Kỹ thuật dồn dịch bộ nhớ (Compaction)
* Phân trang (Paging):
  * Bảng trang (Page Table Entry - PTE: số khung trang, dirty bit, valid bit, reference bit)
  * Dịch địa chỉ (Address translation)
  * Bảng trang nhiều cấp (Multi-level page tables)
  * Bảng trang nghịch đảo (Inverted page tables)
  * Bộ đệm dịch chuyển nhanh (Translation Lookaside Buffer - TLB)
* Phân đoạn (Segmentation):
  * Bảng phân đoạn (Segment table)
  * Phân đoạn kết hợp phân trang (Intel x86)

---

## Chương 7: Bộ nhớ ảo (Virtual Memory)

* Phân trang theo yêu cầu (Demand paging)
* Xử lý lỗi trang (Page fault handling)
* Sao chép khi ghi (Copy-on-write)
* Các giải thuật thay thế trang:
  * FIFO (Hiện tượng dị thường Belady)
  * Tối ưu (Optimal - MIN)
  * LRU (Ít được sử dụng gần đây nhất)
  * Cơ chế đồng hồ (Clock / Second-Chance)
  * LFU, MFU
* Trì trệ hệ thống (Thrashing) và mô hình tập làm việc (working set model)
* Lựa chọn kích thước trang (Page size)
* Phạm vi và độ bao phủ của TLB
* Tệp ánh xạ bộ nhớ (Memory-mapped files)

---

## Chương 8: Hệ thống Tệp tin (File Systems)

* Khái niệm Tệp: thuộc tính, thao tác, các kiểu tệp
* Cấu trúc thư mục: một cấp, hai cấp, cấu trúc cây, đồ thị không chu trình, đồ thị tổng quát
* Gắn kết hệ thống tệp (File system mounting)
* Các phương pháp cấp phát tệp:
  * Cấp phát liên tục (Contiguous allocation)
  * Cấp phát liên kết (Linked allocation - FAT)
  * Cấp phát chỉ mục (Indexed allocation - i-node)
* Quản lý không gian trống:
  * Vector bit (Bit vector)
  * Danh sách liên kết (Linked list)
  * Gom nhóm (Grouping)
  * Đếm (Counting)
* Lập lịch đĩa (Disk scheduling):
  * FCFS, SSTF, SCAN, C-SCAN, LOOK, C-LOOK
* Bố cục hệ thống tệp (boot block, superblock, các khối inode, các khối dữ liệu)
* Hệ thống tệp ghi nhật ký (Journaling) và cấu trúc ghi nhật ký (log-structured)
* Ví dụ hệ thống tệp: ext4, NTFS, FAT32

---

## Chương 9: Hệ thống Vào/Ra (Input/Output Systems)

* Phần cứng vào/ra: các cổng (ports), các bus, bộ điều khiển (controllers)
* Thăm dò (Polling) so với Ngắt (Interrupts)
* Truy cập bộ nhớ trực tiếp (Direct Memory Access - DMA)
* Các tầng phần mềm vào/ra: trình xử lý ngắt, trình điều khiển thiết bị (device drivers), vào/ra độc lập thiết bị, vào/ra cấp người dùng
* Cơ chế đệm (Buffering), bộ đệm ẩn (caching), spooling
* Thiết bị dạng khối (Block devices) và thiết bị dạng ký tự (Character devices)
* Cấu trúc đĩa (tracks, cylinders, sectors)
* Các cấp độ RAID (0, 1, 5, 6, 10)

---

## Chương 10: An ninh và Bảo vệ (Security and Protection)

* Các mục tiêu an ninh: tính bảo mật (confidentiality), tính toàn vẹn (integrity), tính sẵn sàng (availability)
* Xác thực: mật khẩu, sinh trắc học, xác thực hai yếu tố (2FA)
* Kiểm soát truy cập: DAC, MAC, RBAC
* Cơ chế bảo vệ: danh sách quyền (capabilities), ma trận truy cập (access matrix)
* Nguyên tắc đặc quyền tối thiểu (Principle of least privilege)
* Phần mềm độc hại: virus, worm, trojan, rootkit
* Tấn công tràn bộ đệm (Buffer overflow) và các cơ chế phòng vệ
* Cơ bản về mã hóa trong an ninh hệ điều hành (mã hóa toàn bộ đĩa)

---

## Chương 11: Ảo hóa và Điện toán đám mây

* Khái niệm Máy ảo (VM): trình giám sát hypervisors (Loại 1, Loại 2)
* Ảo hóa (Virtualization) so với Mô phỏng (Emulation)
* Cận ảo hóa (Para-virtualization) và ảo hóa hỗ trợ bằng phần cứng
* Container so với Máy ảo (Docker, LXC)
* Các mô hình điện toán đám mây: IaaS, PaaS, SaaS
* Các thách thức: tính cô lập, hiệu năng, an ninh

---

## Chương 12: Hệ điều hành Phân tán (Tổng quan)

* Các mô hình hệ phân tán (Client-Server, Peer-to-Peer)
* Lời gọi thủ tục từ xa (Remote Procedure Call - RPC) và truyền thông điệp (message passing)
* Tính minh bạch mạng (Network transparency) và đặt tên
* Đồng bộ hóa phân tán (Lamport clocks, vector clocks)
* Loại trừ tương hỗ phân tán (tập trung, phân tán, dựa trên thẻ bài/token-based)
* Hệ thống tệp phân tán (NFS, AFS)
* Đồng thuận phân tán (Consensus: Paxos, Raft - mặt khái niệm)

---

## Chương 13: Các chủ đề Nâng cao

* Hệ điều hành thời gian thực (RTOS): VxWorks, FreeRTOS
* Hệ điều hành nhúng (Embedded OS)
* Các tính năng OS di động (quản lý năng lượng, cảm biến)
* Phân tích hiệu năng hệ thống (perf, DTrace)
* Khả năng chịu lỗi (Fault tolerance) và tạo điểm khôi phục (checkpointing)

---

## Ủng hộ (Support)

Nếu đề cương này giúp bạn làm chủ được kiến thức Hệ điều hành, hãy cân nhắc tặng một ngôi sao ⭐ cho repo nhé. Các đóng góp, sửa lỗi và ý tưởng dự án bổ sung luôn được chào đón.
