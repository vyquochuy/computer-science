# MẠNG MÁY TÍNH (COMPUTER NETWORKS)

## Chương 1: Giới thiệu về Mạng máy tính (Introduction to Computer Networks)

* Định nghĩa và mục tiêu của mạng máy tính
* Các tiêu chí đánh giá mạng: hiệu năng (performance), độ tin cậy (reliability), bảo mật (security)
* Các phân loại mạng: LAN, MAN, WAN, PAN, WLAN
* Sơ đồ cấu trúc mạng (Network topologies): Bus, Star (Sao), Ring (Vòng), Mesh (Lưới), Hybrid (Hỗn hợp)
* Các mô hình mạng (Network models):
  * Khách - Chủ (Client-Server)
  * Ngang hàng (Peer-to-Peer - P2P)

---

## Chương 2: Các mô hình mạng (OSI và TCP/IP) (Network Models)

### Mô hình OSI (OSI Model)

* 7 tầng và chức năng tương ứng:
  * Vật lý (Physical), Liên kết dữ liệu (Data Link), Mạng (Network), Giao vận (Transport), Phiên (Session), Trình diễn (Presentation), Ứng dụng (Application)
* Quá trình đóng gói (Encapsulation) và giải đóng gói (Decapsulation)
* Các giao thức tại mỗi tầng

### Mô hình TCP/IP (TCP/IP Model)

* 4 tầng:
  * Giao tiếp mạng (Network Interface), Internet, Giao vận (Transport), Ứng dụng (Application)
* Ánh xạ với mô hình OSI
* Sự khác biệt giữa OSI và TCP/IP

---

## Chương 3: Tầng Vật lý (Physical Layer)

* Phương tiện truyền dẫn (Transmission media):
  * Hữu tuyến (Guided): Cáp xoắn đôi (Twisted Pair), Cáp đồng trục (Coaxial), Cáp quang (Optical Fiber)
  * Vô tuyến (Unguided): Sóng vô tuyến (Radio waves), Sóng cực ngắn (Microwaves), Tia hồng ngoại (Infrared)
* Các loại tín hiệu (Signal types):
  * Tương tự (Analog) và Số (Digital)
* Các chế độ truyền dẫn (Transmission modes):
  * Đơn công (Simplex), Bán song công (Half-duplex), Song công toàn phần (Full-duplex)
* Các kỹ thuật mã hóa đường truyền (Line coding techniques):
  * NRZ, Manchester, Bipolar
* Các khái niệm về tốc độ dữ liệu (Data rate):
  * Định lý Nyquist
  * Khả năng kênh truyền Shannon (Shannon capacity)

---

## Chương 4: Tầng Liên kết dữ liệu (Data Link Layer)

* Các chức năng chính:
  * Phân khung (Framing)
  * Phát hiện và sửa lỗi (Error detection and correction)
  * Kiểm soát luồng (Flow control)
  * Kiểm soát truy cập (Access control)

### Phát hiện lỗi (Error Detection)

* Kiểm tra chẵn lẻ (Parity Check)
* Mã kiểm tổng (Checksum)
* Mã kiểm tra dư thừa vòng (CRC)
* Mã Hamming (Hamming Code)

### Các giao thức kiểm soát luồng & lỗi (Flow & Error Control Protocols)

* Stop-and-Wait (Dừng và chờ)
* Cửa sổ trượt (Sliding Window)
* Cơ chế ARQ (Automatic Repeat Request):
  * Stop-and-Wait ARQ
  * Go-Back-N
  * Selective Repeat (Truyền lại có chọn lọc)

### Phân lớp MAC (MAC Sub-layer)

* Địa chỉ vật lý (MAC addressing)
* Các giao thức truy cập kênh truyền (Channel access protocols):
  * ALOHA (ALOHA thuần - Pure, ALOHA phân khe - Slotted)
  * CSMA/CD
  * CSMA/CA

---

## Chương 5: Tầng Mạng (Network Layer)

* Các chức năng chính:
  * Định danh logic (Logical addressing)
  * Định tuyến (Routing)
  * Chuyển tiếp gói tin (Packet forwarding)

### Địa chỉ IP (IP Addressing)

* IPv4:
  * Phân lớp địa chỉ (Lớp A, B, C, D, E)
  * Chia mạng con (Subnetting)
  * CIDR (Định tuyến liên miền không phân lớp) / Gộp mạng con (Supernetting)
* Kiến thức cơ bản về IPv6

### Các thuật toán định tuyến (Routing Algorithms)

* Thuật toán vectơ khoảng cách (Distance Vector)
* Thuật toán trạng thái liên kết (Link State)
* Thuật toán tràn ngập (Flooding)

### Các giao thức định tuyến (Routing Protocols)

* Giao thức RIP
* Giao thức OSPF
* Khái niệm cơ bản về giao thức BGP

### Các chủ đề bổ sung

* Cơ chế phân mảnh gói tin (Fragmentation)
* Dịch địa chỉ mạng (NAT)
* Giao thức thông điệp điều khiển Internet (ICMP)

---

## Chương 6: Tầng Giao vận (Transport Layer)

* Các chức năng chính:
  * Truyền thông tiến trình - tiến trình (Process-to-process communication)
  * Phân đoạn và tái hợp gói tin (Segmentation and reassembly)
  * Kiểm soát lỗi (Error control)
  * Kiểm soát luồng (Flow control)
  * Kiểm soát tắc nghẽn (Congestion control)

### Giao thức TCP

* Giao thức hướng kết nối (Connection-oriented protocol)
* Truyền thông tin cậy (Reliable communication)
* Bắt tay 3 bước (3-way handshake)
* Kiểm soát luồng (Cửa sổ trượt - Sliding Window)
* Kiểm soát tắc nghẽn (Congestion control):
  * Khởi đầu chậm (Slow Start)
  * Tránh tắc nghẽn (Congestion Avoidance)
  * Truyền lại nhanh (Fast Retransmit)
  * Khôi phục nhanh (Fast Recovery)

### Giao thức UDP

* Giao thức không kết nối (Connectionless protocol)
* Không tin cậy nhưng tốc độ nhanh (Unreliable but fast)
* Các trường hợp sử dụng phù hợp

---

## Chương 7: Tầng Ứng dụng (Application Layer)

* Giao thức HTTP / HTTPS
* Giao thức FTP
* Giao thức SMTP
* Giao thức POP3 / IMAP
* Hệ thống phân giải tên miền (DNS)
* Giao thức cấu hình động máy chủ (DHCP)

### Các khái niệm cốt lõi

* Kiến trúc Khách - Chủ (Client-server architecture)
* Cấu trúc đường dẫn URL (URL structure)
* Cơ chế Cookies và Sessions

---

## Chương 8: Bảo mật mạng (Network Security)

* Các mục tiêu bảo mật:
  * Tính bí mật (Confidentiality)
  * Tính toàn vẹn (Integrity)
  * Tính xác thực (Authentication)
  * Tính chống chối bỏ (Non-repudiation)

### Mã hóa học (Cryptography)

* Mã hóa khóa đối xứng (Symmetric key encryption)
* Mã hóa khóa bất đối xứng (Asymmetric key encryption)
* Các hàm băm (Hash functions)

### Các giao thức bảo mật

* Giao thức SSL/TLS
* Giao thức HTTPS

### Các kiểu tấn công mạng phổ biến

* Tấn công giả mạo (Phishing)
* Tấn công kẻ đứng giữa (Man-in-the-middle)
* Tấn công từ chối dịch vụ (DoS / DDoS)

### Các cơ chế phòng vệ

* Tường lửa (Firewalls)
* Hệ thống phát hiện xâm nhập (Intrusion Detection Systems - IDS)

---

## Chương 9: Kiểm soát tắc nghẽn và Đảm bảo chất lượng dịch vụ (Congestion Control and QoS)

* Các nguyên nhân gây tắc nghẽn mạng
* Kỹ thuật điều phối lưu lượng (Traffic shaping):
  * Thuật toán xô rò (Leaky Bucket)
  * Thuật toán xô thẻ bài (Token Bucket)

### Chất lượng dịch vụ (Quality of Service - QoS)

* Độ trễ (Delay)
* Độ biến động trễ (Jitter)
* Băng thông khả dụng (Bandwidth)

---

## Chương 10: Mạng không dây và Mạng di động (Wireless and Mobile Networks)

* Khái niệm cơ bản về truyền thông không dây
* Mạng thông tin di động (Cellular networks)

### Wi-Fi (IEEE 802.11)

* Kiến trúc mạng không dây
* Cơ chế kiểm soát truy cập CSMA/CA

### Mobile IP (Tổng quan cơ bản)

---

## Chương 11: Các bài tập tính toán quan trọng (Important Numerical Topics)

* Các bài toán chia mạng con (Subnetting problems)
* Trễ truyền dẫn (Transmission delay) và Trễ lan truyền (Propagation delay)
* Tích số băng thông - độ trễ (Bandwidth-delay product)
* Cách tính toán kiểm tra dư thừa vòng CRC
* Các bài toán liên quan đến cửa sổ trượt (Sliding window)

---

## Chương 12: Các chủ đề phỏng vấn quan trọng (Important Interview Topics)

* So sánh chi tiết TCP vs UDP
* So sánh chi tiết OSI vs TCP/IP
* So sánh chi tiết HTTP vs HTTPS
* Cơ chế hoạt động của hệ thống phân giải tên miền DNS
* Quá trình bắt tay 3 bước (3-way handshake) và Ngắt kết nối 4 bước (4-way termination)
* Cơ chế hoạt động của Dịch địa chỉ mạng NAT
* Phân biệt giữa các thiết bị Hub, Switch và Router

---
## ⭐ Ủng hộ dự án

Nếu kho lưu trữ này mang lại giá trị cho quá trình học tập của bạn, hãy cân nhắc tặng ⭐ để bày tỏ sự ủng hộ đối với dự án.
