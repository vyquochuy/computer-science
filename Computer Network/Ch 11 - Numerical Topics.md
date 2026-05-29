# Chương 11: Các bài tập tính toán trong Mạng máy tính (Numerical Topics)

Tài liệu này cung cấp một cái nhìn tổng quan có cấu trúc về các dạng bài tập tính toán cơ bản và thiết thực nhất trong mạng máy tính. Nó được thiết kế nhằm giúp học viên, giáo viên và các kỹ sư thực hành rèn luyện kỹ năng giải quyết các vấn đề kỹ thuật thông qua các ví dụ thực tế và lời giải chi tiết. Năm chủ đề lớn sau đây sẽ được đề cập chuyên sâu.

## Các chủ đề được đề cập

1. **Chia mạng con (Subnetting):** Định lý địa chỉ IPv4, tính toán mặt nạ mạng con (subnet mask), định tuyến không phân lớp CIDR, chia mạng con độ dài biến đổi VLSM, xác định địa chỉ mạng và địa chỉ quảng bá.
2. **Trễ truyền dẫn và Trễ lan truyền (Transmission & Propagation Delay):** Tính toán thời gian đẩy gói dữ liệu lên kênh truyền, thời gian tín hiệu lan truyền vật lý và tổng độ trễ hệ thống.
3. **Tích số băng thông - độ trễ (Bandwidth–Delay Product - BDP):** Tính dung lượng tối đa của một đường truyền dựa trên thời gian trễ khứ hồi RTT, xác định yêu cầu kích thước bộ nhớ đệm và tối ưu hóa cửa sổ trượt.
4. **Kiểm tra dư thừa vòng (CRC):** Các phép toán đa thức nhị phân, phép chia modulo-2, quy trình tạo mã kiểm lỗi CRC và sử dụng đa thức tạo.
5. **Giao thức cửa sổ trượt (Sliding Window Protocol):** Tính toán kích thước cửa sổ tối đa cho các giao thức Quay lại N (Go-Back-N), Truyền lại có chọn lọc (Selective Repeat), không gian số thứ tự và tốc độ thông lượng (throughput) thực tế.

Mỗi chủ đề sẽ bao gồm:
- Các công thức cốt lõi kèm theo chú thích ký hiệu rõ ràng.
- Các ví dụ minh họa từng bước giải chi tiết.
- Các bài tập tự luyện (lời giải chi tiết được cung cấp tại thư mục `/solutions` của kho lưu trữ).
- Các kịch bản Python mẫu hỗ trợ tự động hóa tính toán (nằm tại thư mục `/scripts`).

---

## 1. Chia mạng con (Subnetting)

### Các công thức cốt lõi
- Số lượng mạng con tạo ra = $2^s$, với $s$ là số bit mượn từ phần máy chủ.
- Số máy trạm khả dụng trong mỗi mạng con = $2^h - 2$, với $h$ là số bit còn lại của phần máy chủ.
- Mặt nạ mạng con mới = mặt nạ mạng gốc cộng thêm $s$ bit 1 ở phần mạng.
- Kích thước khối (bước nhảy địa chỉ giữa các mạng con) = $2^h$.

### Bài tập ví dụ
**Đề bài:** Cho dải mạng gốc `192.168.1.0/24`. Thực hiện chia dải mạng này thành 4 mạng con.  
**Lời giải chi tiết:**  
- Số bit cần mượn $s = \log_2(4) = 2$ bit.  
- Mặt nạ mạng con mới = `/26` (tương ứng biểu diễn thập phân là `255.255.255.192`).  
- Số bit máy chủ còn lại $h = 8 - 2 = 6$ bit $\rightarrow$ Số máy trạm khả dụng trên mỗi mạng con = $2^6 - 2 = 62$ máy.  
- Địa chỉ của 4 mạng con thu được lần lượt là: `192.168.1.0`, `192.168.1.64`, `192.168.1.128`, `192.168.1.192`.

---

## 2. Trễ truyền dẫn và Trễ lan truyền (Transmission & Propagation Delay)

### Các công thức cốt lõi
- Trễ truyền dẫn (Transmission delay - $d_{trans}$) = $\frac{L}{R}$  
  (Trong đó: $L$ = kích thước gói tin tính bằng bit, $R$ = tốc độ truyền của đường truyền tính bằng bps)
- Trễ lan truyền (Propagation delay - $d_{prop}$) = $\frac{d}{s}$  
  (Trong đó: $d$ = khoảng cách vật lý của đường truyền, $s$ = vận tốc lan truyền tín hiệu trong môi trường vật lý, thông thường đạt xấp xỉ $2 \times 10^8\text{ m/s}$ đối với dây đồng hoặc cáp quang)
- Tổng độ trễ hệ thống (Total latency) = Trễ truyền dẫn + Trễ lan truyền + Trễ xếp hàng + Trễ xử lý (trễ xếp hàng và xử lý thường được lược bỏ trong các bài toán lý thuyết cơ bản).

### Bài tập ví dụ
**Đề bài:** Truyền gói tin kích thước 1500 byte qua đường truyền tốc độ $10\text{ Mbps}$, khoảng cách địa lý giữa hai điểm là $2000\text{ km}$, vận tốc lan truyền tín hiệu trong dây dẫn là $2 \times 10^8\text{ m/s}$.  
**Lời giải chi tiết:**  
- Đổi đơn vị kích thước gói tin sang bit: $L = 1500 \times 8 = 12,000\text{ bit}$.  
- Trễ truyền dẫn $d_{trans} = \frac{12,000}{10 \times 10^6} = 0.0012\text{ giây} = 1.2\text{ ms}$.  
- Trễ lan truyền $d_{prop} = \frac{2000 \times 10^3}{2 \times 10^8} = 0.01\text{ giây} = 10\text{ ms}$.  
- Tổng độ trễ truyền dẫn đầu-cuối = $1.2 + 10 = 11.2\text{ ms}$.

---

## 3. Tích số băng thông - độ trễ (Bandwidth–Delay Product - BDP)

### Công thức cốt lõi
- Tích số băng thông - độ trễ (BDP) = Băng thông (bps) $\times$ Thời gian trễ khứ hồi RTT (giây)  
  Đơn vị tính: bit (hoặc chia cho 8 để đổi sang byte).

### Ý nghĩa kỹ thuật
BDP đại diện cho dung lượng dữ liệu tối đa có thể nằm đồng thời "trên đường truyền" (đang bay) trước khi bên gửi nhận được thông điệp xác nhận phản hồi ACK đầu tiên. Chỉ số này quyết định kích thước cửa sổ tối thiểu cần thiết để tận dụng hết 100% hiệu năng của đường truyền.

### Bài tập ví dụ
**Đề bài:** Cho đường truyền có băng thông $100\text{ Mbps}$, thời gian trễ khứ hồi $RTT = 50\text{ ms}$.  
**Lời giải chi tiết:**  
$BDP = 100 \times 10^6 \times 0.05 = 5 \times 10^6\text{ bit} = 625,000\text{ byte}$.  
Để tận dụng tối đa băng thông đường truyền, kích thước cửa sổ của bên gửi bắt buộc phải đạt tối thiểu $625,000\text{ byte}$ (tương đương khoảng 417 gói tin có kích thước tiêu chuẩn 1500 byte).

---

## 4. Kiểm tra dư thừa vòng (Cyclic Redundancy Check - CRC)

### Khái niệm cốt lõi
- CRC sử dụng phép chia đa thức hệ nhị phân (phép chia modulo-2) để tạo ra mã kiểm tổng checksum.
- Bên gửi thêm vào cuối chuỗi dữ liệu gốc $n$ bit kiểm tra (với $n$ là bậc cao nhất của đa thức tạo).
- Bên nhận chia dữ liệu nhận được cho cùng đa thức tạo; nếu số dư bằng 0 nghĩa là dữ liệu trọn vẹn không lỗi.

### Quy trình giải bài toán
1. Thêm $n$ bit 0 vào cuối chuỗi dữ liệu gốc (với $n$ là bậc cao nhất của đa thức tạo).
2. Thực hiện phép chia chuỗi dữ liệu đã mở rộng cho đa thức tạo sử dụng phép toán XOR nhị phân (phép chia modulo-2).
3. Phần số dư thu được (có độ dài $n$ bit) chính là mã kiểm lỗi CRC.
4. Chuỗi dữ liệu thực tế truyền đi = Chuỗi dữ liệu gốc ghép thêm mã CRC ở cuối.
5. Bên nhận thực hiện phép chia chuỗi nhận được cho cùng đa thức tạo; nếu số dư khác 0, kết luận gói tin bị lỗi.

### Bài tập ví dụ
**Đề bài:** Chuỗi dữ liệu gốc = `110101`, Đa thức tạo = `1011` (bậc 3).  
**Lời giải chi tiết:**  
- Thêm 3 bit 0 vào cuối dữ liệu $\rightarrow$ `110101000`.  
- Thực hiện phép chia cho `1011` dùng XOR:  
  (Quá trình chia nhị phân XOR tuần tự cho ra số dư cuối cùng là `011`).  
- Khung dữ liệu truyền đi hoàn chỉnh: `110101011`.  
- Tại phía nhận, thực hiện chia `110101011` cho `1011` thu được số dư `000` $\rightarrow$ xác nhận dữ liệu chính xác không lỗi.

---

## 5. Giao thức cửa sổ trượt (Sliding Window Protocol)

### Các công thức cốt lõi
- Kích thước cửa sổ gửi tối đa của giao thức Quay lại N (Go‑Back‑N) = $2^n - 1$, với $n$ là số bit dùng để đánh số thứ tự trong tiêu đề gói tin.
- Kích thước cửa sổ gửi tối đa của giao thức Truyền lại có chọn lọc (Selective Repeat) = $2^{n-1}$.
- Tốc độ thông lượng thực tế (Throughput) = $\frac{\text{Kích thước cửa sổ} \times \text{Kích thước gói tin}}{RTT}$, công thức này áp dụng khi kích thước cửa sổ $\le$ BDP.

### Bài tập ví dụ (Cho giao thức Go-Back-N)
**Đề bài:** Sử dụng trường số thứ tự 3-bit (đánh số từ 0 đến 7), thời gian trễ khứ hồi $RTT = 200\text{ ms}$, kích thước gói tin = 1000 byte, băng thông đường truyền vật lý = $2\text{ Mbps}$.  
**Lời giải chi tiết:**  
- Kích thước cửa sổ gửi tối đa khả dụng = $2^3 - 1 = 7$ gói tin.  
- Đổi đơn vị tính BDP: $BDP = 2 \times 10^6 \times 0.2 = 400,000\text{ bit} = 50,000\text{ byte} = 50$ gói tin (kích thước 1000 byte).  
- Do kích thước cửa sổ thực tế (7 gói) nhỏ hơn rất nhiều so với dung lượng tối đa đường truyền BDP (50 gói), đường truyền đang hoạt động không hết công suất.  
- Tốc độ thông lượng thực tế đạt được = $\frac{7 \times 1000 \times 8}{0.2} = 280,000\text{ bps} = 0.28\text{ Mbps}$.

---

## Cách sử dụng tài liệu

1. Nghiên cứu kỹ lưỡng các công thức và ví dụ minh họa từng bước giải chi tiết trong mỗi phần.
2. Tự mình thực hành giải quyết các bài tập tự luyện tương ứng nằm tại thư mục `/problems` của kho lưu trữ.
3. Đối chiếu và kiểm tra lại kết quả bài giải của mình thông qua các hướng dẫn giải chi tiết tại thư mục `/solutions`.
4. Có thể chạy trực tiếp các kịch bản lệnh Python trong thư mục `/scripts` để tự động hóa tính toán hoặc kiểm thử các trường hợp đặc biệt.

---
