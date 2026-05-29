# Chương 12: Các chủ đề phỏng vấn quan trọng (Important Interview Topics)

Chương này tổng hợp các kiến thức cốt lõi và kinh điển nhất về mạng máy tính thường xuyên được thảo luận và đặt câu hỏi trong các cuộc phỏng vấn kỹ thuật tuyển dụng lập trình viên, kỹ sư mạng. Mỗi chủ đề đều được trình bày một cách ngắn gọn, súc tích, làm rõ những điểm khác biệt bản chất và đúc kết các nhận thức thực tiễn.

---

## 1. So sánh chi tiết bộ giao thức TCP và UDP (TCP vs UDP)

Giao thức TCP (Transmission Control Protocol) và UDP (User Datagram Protocol) là hai giao thức cốt lõi và phổ biến nhất ở Tầng Giao vận.

| Đặc điểm so sánh | Giao thức TCP | Giao thức UDP |
|------------------|-----------------------------------------------|-----------------------------------------------|
| **Kết nối** | Hướng kết nối (Connection-oriented - yêu cầu bắt tay 3 bước) | Không kết nối (Connectionless) |
| **Độ tin cậy** | Đảm bảo tin cậy tuyệt đối (xác nhận ACK, truyền lại, đánh số thứ tự byte) | Không đảm bảo (truyền nỗ lực tối đa, có thể mất gói) |
| **Kiểm soát luồng** | Có hỗ trợ (cơ chế cửa sổ trượt Sliding window) | Không hỗ trợ |
| **Kiểm soát tắc nghẽn** | Có hỗ trợ (thuật toán Slow start, Congestion avoidance,...) | Không hỗ trợ |
| **Độ chính xác thứ tự** | Đảm bảo các phân đoạn dữ liệu đến đích đúng thứ tự sắp xếp | Không đảm bảo thứ tự truyền |
| **Chi phí tiêu đề (Overhead)** | Cao hơn (tiêu đề tối thiểu dài 20 byte) | Rất thấp (tiêu đề cố định dài 8 byte) |
| **Tốc độ truyền tải** | Chậm hơn (do mất thời gian thiết lập kết nối và kiểm tra lỗi) | Cực kỳ nhanh và có độ trễ thấp tối đa |
| **Các ứng dụng phổ biến** | HTTP, HTTPS, FTP, SSH, gửi nhận Email (SMTP/POP/IMAP) | DNS, cuộc gọi VoIP, Live streaming, Game online, DHCP |

**Nhận thức mấu chốt:** Hãy lựa chọn sử dụng TCP khi tính toàn vẹn và thứ tự chính xác của dữ liệu là bắt buộc (như tải file, duyệt web, giao dịch); lựa chọn UDP khi ưu tiên tốc độ truyền tải cực nhanh và độ trễ thấp là sống còn, chấp nhận một tỷ lệ mất mát gói dữ liệu nhỏ không đáng kể.

---

## 2. So sánh mô hình OSI và mô hình TCP/IP

Cả hai mô hình đều dùng để mô tả cấu trúc phân tầng của bộ giao thức mạng, nhưng có sự khác biệt về độ chi tiết phân chia và nguồn gốc lịch sử phát triển.

- **Mô hình tham chiếu OSI (7 tầng):** Vật lý (1), Liên kết dữ liệu (2), Mạng (3), Giao vận (4), Phiên (5), Trình diễn (6), Ứng dụng (7).
- **Mô hình thực tế TCP/IP (4 tầng):** Truy cập mạng (1 - gộp lớp 1 và 2 của OSI), Internet (2 - tương đương lớp 3 OSI), Giao vận (3 - tương đương lớp 4 OSI), Ứng dụng (4 - gộp lớp 5, 6, 7 của OSI).

| Các lớp OSI tương ứng | Tầng trong mô hình TCP/IP | Các giao thức / Công nghệ tiêu biểu |
|--------------------|---------------------|----------------------------|
| 7 – Ứng dụng (Application) | Ứng dụng (Application) | HTTP, HTTPS, FTP, DNS, SMTP, IMAP |
| 6 – Trình diễn (Presentation) | Ứng dụng (Application) | TLS/SSL (một phần), bảng mã ASCII, JPEG |
| 5 – Phiên (Session) | Ứng dụng (Application) | NetBIOS, gọi thủ tục từ xa RPC |
| 4 – Giao vận (Transport) | Giao vận (Transport) | TCP, UDP |
| 3 – Mạng (Network) | Internet | IPv4, IPv6, ICMP, ARP |
| 2 – Liên kết dữ liệu (Data Link) | Truy cập mạng (Network Interface) | Ethernet (802.3), Wi‑Fi (802.11), PPP |
| 1 – Vật lý (Physical) | Truy cập mạng (Network Interface) | Chuẩn cáp đồng RJ45, sợi quang, sóng vô tuyến |

**Nhận thức mấu chốt:** OSI là một mô hình tham chiếu lý thuyết dùng trong giảng dạy và thiết kế; TCP/IP là bộ mô hình tiêu chuẩn thực tế đang trực tiếp vận hành mạng Internet ngày nay.

---

## 3. So sánh chi tiết HTTP và HTTPS

| Đặc điểm | Giao thức HTTP | Giao thức HTTPS |
|------------------|-------------------------------------|-------------------------------------------------|
| **Tên đầy đủ** | HyperText Transfer Protocol | HyperText Transfer Protocol Secure (HTTP bảo mật) |
| **Mật mã hóa dữ liệu** | Không có (dữ liệu truyền dạng văn bản thô cleartext) | Mã hóa an toàn thông qua giao thức TLS/SSL |
| **Cổng mặc định** | Cổng 80 | Cổng 443 |
| **Mức độ an toàn** | Dễ bị nghe lén dữ liệu, tấn công kẻ đứng giữa MITM | Bảo đảm tính bí mật, toàn vẹn dữ liệu và xác thực |
| **Hiệu năng tốc độ** | Nhanh hơn một chút (không mất tài nguyên mã hóa) | Chậm hơn một chút (mất thời gian bắt tay và mã hóa) |
| **Ảnh hưởng SEO web** | Bị đánh giá thấp (trình duyệt cảnh báo không an toàn) | Ưu tiên tăng thứ hạng tìm kiếm, tạo độ tin cậy |
| **Chứng chỉ số yêu cầu** | Không yêu cầu | Bắt buộc phải có chứng chỉ số được ký bởi tổ chức CA |

**Cơ chế hoạt động của HTTPS:**
- Máy chủ xuất trình chứng chỉ kỹ thuật số (digital certificate) cho trình duyệt.
- Trình duyệt xác thực chứng chỉ với các tổ chức CA đáng tin cậy tích hợp sẵn.
- Hai bên thực hiện bắt tay mã hóa khóa bất đối xứng để thống nhất một khóa phiên làm việc đối xứng (symmetric session key).
- Toàn bộ dữ liệu trao đổi tiếp theo được mã hóa đối xứng bằng khóa phiên này để truyền tốc độ cao.

**Nhận thức mấu chốt:** Luôn bắt buộc triển khai giao thức HTTPS trên tất cả các trang web môi trường thực tế để bảo vệ thông tin cá nhân và dữ liệu thanh toán của người dùng.

---

## 4. Cơ chế hoạt động của hệ thống phân giải tên miền DNS

DNS đóng vai trò dịch các tên miền dễ đọc (ví dụ: `google.com`) thành các địa chỉ IP đích để máy tính kết nối.

**Các bước phân giải tên miền đệ quy (recursive query):**
1. Trình duyệt kiểm tra bộ đệm cục bộ (của trình duyệt và hệ điều hành OS).
2. Nếu không tìm thấy, truy vấn được gửi tới **Bộ phân giải DNS cục bộ (Recursive Resolver)** (thường do nhà mạng ISP cung cấp hoặc DNS công cộng như Cloudflare 1.1.1.1, Google 8.8.8.8).
3. Bộ phân giải Resolver sẽ hỏi **Máy chủ tên miền gốc (Root nameserver)** $\rightarrow$ trả về địa chỉ máy chủ quản lý tên miền cấp cao nhất (TLD).
4. Resolver tiếp tục hỏi **Máy chủ TLD (TLD nameserver)** (ví dụ đuôi `.com`) $\rightarrow$ trả về địa chỉ của máy chủ DNS có thẩm quyền của tên miền.
5. Resolver hỏi **Máy chủ DNS có thẩm quyền (Authoritative nameserver)** của tên miền $\rightarrow$ nhận về địa chỉ IP đích (bản ghi A hoặc AAAA).
6. Resolver phản hồi địa chỉ IP về cho trình duyệt và tự động lưu đệm lại (với thời gian lưu đệm quy định bởi chỉ số TTL).

**Các loại bản ghi DNS quan trọng:**
- **A:** Bản ghi ánh xạ tên miền sang địa chỉ IPv4.
- **AAAA:** Bản ghi ánh xạ tên miền sang địa chỉ IPv6.
- **CNAME:** Bản ghi bí danh tên miền (trỏ một tên miền phụ về tên miền gốc).
- **MX:** Bản ghi chỉ định máy chủ đảm nhận nhận email của tên miền.
- **TXT:** Bản ghi văn bản (thường dùng xác thực cấu hình bảo mật email SPF, DKIM).
- **NS:** Bản ghi chỉ định các máy chủ tên miền có thẩm quyền của tên miền.

**Nhận thức mấu chốt:** DNS hoạt động theo mô hình cơ sở dữ liệu phân tán phân cấp khổng lồ, áp dụng cơ chế lưu trữ đệm (caching) cực kỳ mạnh mẽ ở mọi cấp độ để giảm thiểu tối đa độ trễ truy cập web.

---

## 5. Quá trình Bắt tay 3 bước và Ngắt kết nối 4 bước trong TCP

Đây là hai tiến trình cốt lõi dùng để thiết lập và giải phóng kết nối một cách an toàn, tin cậy trong giao thức TCP.

### Bắt tay 3 bước (3-Way Handshake - SYN, SYN-ACK, ACK)

1. **Khách $\rightarrow$ Chủ: `SYN` (seq=x)**
   - Máy khách gửi một phân đoạn TCP có cờ SYN được kích hoạt, đề xuất số thứ tự bắt đầu là $x$.
2. **Chủ $\rightarrow$ Khách: `SYN-ACK` (seq=y, ack=x+1)**
   - Máy chủ xác nhận đã nhận yêu cầu (ack = x+1), đồng thời gửi lại cờ SYN của mình đề xuất số thứ tự bắt đầu là $y$.
3. **Khách $\rightarrow$ Chủ: `ACK` (ack=y+1)**
   - Máy khách xác nhận đã nhận thông điệp của máy chủ (ack = y+1). Hai bên chính thức chuyển sang trạng thái kết nối thành công (`ESTABLISHED`).

*Chuyển đổi trạng thái:*
- Máy khách: CLOSED $\rightarrow$ SYN_SENT $\rightarrow$ ESTABLISHED
- Máy chủ: LISTEN $\rightarrow$ SYN_RCVD $\rightarrow$ ESTABLISHED

### Ngắt kết nối 4 bước (4-Way Termination - FIN, ACK, FIN, ACK)

Bất kỳ bên nào cũng có quyền chủ động yêu cầu đóng kết nối trước. Giả định máy khách chủ động đóng kết nối:

1. **Khách $\rightarrow$ Chủ: `FIN` (seq=u)**
   - Máy khách gửi thông điệp FIN để thông báo: "Tôi đã hoàn tất việc truyền dữ liệu và muốn đóng kết nối".
2. **Chủ $\rightarrow$ Khách: `ACK` (ack=u+1)**
   - Máy chủ xác nhận đã nhận yêu cầu đóng. Máy chủ chuyển sang trạng thái đóng một nửa (half-closed) – máy chủ vẫn được phép tiếp tục truyền nốt dữ liệu dang dở cho máy khách.
3. **Chủ $\rightarrow$ Khách: `FIN` (seq=v, ack=u+1)**
   - Sau khi máy chủ đã hoàn tất truyền toàn bộ dữ liệu của mình, nó gửi thông điệp FIN để thông báo đóng kết nối ở phía mình.
4. **Khách $\rightarrow$ Chủ: `ACK` (ack=v+1)**
   - Máy khách gửi thông điệp xác nhận đã nhận yêu cầu đóng của máy chủ. Máy khách đợi một khoảng thời gian ngắn quy định (`TIME_WAIT` - thường là 2MSL để đảm bảo ACK cuối đến đích an toàn và dọn sạch các gói tin rác còn bay trên mạng) trước khi chính thức chuyển hẳn về trạng thái đóng hoàn toàn `CLOSED`.

**Nhận thức mấu chốt:** Quá trình bắt tay 3 bước đảm bảo hai bên đồng bộ hóa dữ liệu để thiết lập kênh truyền tin cậy; quá trình ngắt 4 bước đảm bảo giải phóng tài nguyên an toàn cho cả hai bên mà hoàn toàn không lo bị mất mát dữ liệu đang truyền dang dở.

---

## 6. Cơ chế hoạt động của Dịch địa chỉ mạng NAT

NAT cho phép nhiều thiết bị hoạt động trong một mạng nội bộ dùng chung một địa chỉ IP công cộng (Public IP) duy nhất để truy cập ra mạng Internet bên ngoài.

**Các phân loại NAT chính:**
- **NAT tĩnh (Static NAT):** Ánh xạ trực tiếp 1-1 cố định giữa một địa chỉ IP nội bộ với một địa chỉ IP công cộng (thường dùng cấu hình cho các máy chủ dịch vụ).
- **NAT động (Dynamic NAT):** Ánh xạ tự động các IP nội bộ vào một nhóm (pool) các địa chỉ IP công cộng có sẵn khi có nhu cầu kết nối.
- **Dịch địa chỉ cổng PAT (Port Address Translation - NAT Overload):** Ánh xạ nhiều IP nội bộ dùng chung một IP công cộng duy nhất bằng cách gán thêm các số hiệu cổng (port numbers) khác nhau cho từng kết nối. Đây là cơ chế phổ biến nhất được tích hợp trong tất cả các router gia đình hiện nay.

**Cơ chế vận hành của PAT:**
- Thiết bị nội bộ (`192.168.1.10:12345`) gửi gói tin ra ngoài.
- Router nhận được sẽ thay thế IP nguồn nội bộ bằng địa chỉ IP công cộng của router, đồng thời gán cho nó một cổng nguồn ngẫu nhiên duy nhất (ví dụ: `55001`).
- Router ghi chép thông tin này vào **Bảng ánh xạ NAT (NAT table)** để quản lý: `(192.168.1.10:12345) ↔ (IP_công_cộng:55001)`.
- Khi có gói tin phản hồi từ ngoài gửi về cổng `55001` của router, router sẽ tra bảng định vị và dịch ngược lại địa chỉ IP nguồn thành `192.168.1.10:12345` để chuyển tiếp chính xác về máy trạm nội bộ.

**Ưu điểm:** Tiết kiệm đáng kể tài nguyên địa chỉ IPv4 đang cạn kiệt, ẩn giấu cấu trúc mạng nội bộ trước thế giới bên ngoài (đóng vai trò như một cơ chế tường lửa bảo vệ cơ bản).  
**Nhược điểm:** Phá vỡ nguyên lý kết nối trực tiếp đầu-cuối (end-to-end connectivity), gây thêm độ trễ xử lý gói tin và đòi hỏi các kỹ thuật vượt rào phức tạp (NAT traversal như STUN, TURN, ICE) đối với các ứng dụng kết nối trực tiếp P2P như WebRTC hay chơi game online.

---

## 7. Phân biệt bản chất giữa các thiết bị Hub, Switch và Router

Đây là ba thiết bị mạng cơ bản hoạt động ở các tầng khác nhau trong mô hình OSI.

| Đặc điểm so sánh | Thiết bị Hub (Lớp 1) | Thiết bị Switch (Lớp 2) | Thiết bị Router (Lớp 3) |
|------------------|------------------------------|------------------------------|--------------------------------|
| **Lớp OSI hoạt động** | Vật lý (Physical) | Liên kết dữ liệu (Data Link) | Mạng (Network) |
| **Mức độ thông minh** | Không có (chỉ là bộ lặp tín hiệu điện thô sơ) | Khá cao (tự học và lưu bảng địa chỉ MAC) | Rất cao (xử lý định tuyến và tìm đường đi tối ưu) |
| **Cơ chế truyền tin** | Quảng bá (Broadcasting) dội dữ liệu ra tất cả các cổng vật lý | Chỉ gửi dữ liệu chính xác đến cổng của thiết bị đích | Tra bảng định tuyến để chuyển tiếp gói tin qua các dải mạng |
| **Miền xung đột** | Dùng chung một miền xung đột duy nhất cho toàn bộ các cổng | Mỗi cổng vật lý là một miền xung đột độc lập hoàn toàn | Mỗi cổng vật lý là một miền xung đột độc lập hoàn toàn |
| **Miền quảng bá** | Dùng chung một miền quảng bá duy nhất | Dùng chung một miền quảng bá duy nhất (trừ khi chia VLAN) | Ngăn chặn và phân tách các miền quảng bá giữa các cổng |
| **Loại địa chỉ xử lý** | Không xử lý địa chỉ (chỉ là tín hiệu xung điện) | Địa chỉ vật lý MAC | Địa chỉ logic IP |
| **Ứng dụng điển hình** | Đã lỗi thời, không còn sử dụng | Kết nối các thiết bị nội bộ trong cùng mạng LAN | Liên kết mạng nội bộ LAN ra mạng diện rộng WAN / Internet |

**Nhận thức mấu chốt:** Hãy trang bị Switch khi muốn mở rộng cổng kết nối và tối ưu hóa hiệu năng truyền tải dữ liệu nội bộ trong mạng LAN; sử dụng Router khi có nhu cầu liên kết các dải mạng khác nhau và kết nối ra Internet.

---
