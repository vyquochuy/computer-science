# Chương 9: Hệ thống Vào/Ra (Input/Output Systems)

Vào/Ra (*Input/Output - I/O*) là phương thức duy nhất để máy tính có thể giao tiếp và tương tác với thế giới bên ngoài. Chương này giới thiệu các thành phần phần cứng và phần mềm điều khiển quá trình vào/ra dữ liệu, đi từ bộ điều khiển thiết bị ngoại vi, xử lý ngắt phần cứng, cơ chế truy cập bộ nhớ trực tiếp (DMA), cho đến các mảng đĩa cứng dự phòng (RAID).

---

## Phần cứng Vào/Ra: Cổng, Bus, Bộ điều khiển

Các thiết bị ngoại vi kết nối với máy tính thông qua một hệ phân cấp các thành phần phần cứng.

### Cổng (Ports)
Một **cổng** (*port*) là một đầu nối vật lý trên máy tính (ví dụ: cổng USB, cổng mạng Ethernet, cổng HDMI) cung cấp giao diện điện học và logic cho thiết bị ngoại vi. Mỗi cổng được gắn với một hoặc nhiều địa chỉ vào/ra (I/O addresses) cụ thể.

### Bus hệ thống
Một **bus** là một tập hợp các đường dây dẫn điện chia sẻ chung giữa nhiều thiết bị ngoại vi để truyền thông tin. Các loại bus phổ biến:

| Loại Bus | Mục đích sử dụng điển hình | Tốc độ truyền tải |
| :--- | :--- | :--- |
| **PCIe** | Kết nối card đồ họa, ổ cứng NVMe SSD | Vài GB/s |
| **SATA** | Kết nối ổ đĩa cứng HDD, ổ SSD | 6 Gb/s |
| **USB** | Bàn phím, chuột, ổ flash USB | Lên đến 40 Gb/s (chuẩn USB4) |
| **Memory bus** | Kết nối trực tiếp CPU ↔ bộ nhớ RAM | Siêu cao (50-100 GB/s) |

Bus hệ thống (*System bus*) kết nối trực tiếp CPU, bộ nhớ RAM và các cầu nối vào/ra. Bus ngoại vi (*I/O bus* - ví dụ: PCIe) dùng để kết nối các bộ điều khiển thiết bị ngoại vi.

### Bộ điều khiển (Controllers / Adapters)
Một **bộ điều khiển** (*controller*) là một chip vi mạch hoặc mạch điện tử chịu trách nhiệm quản lý trực tiếp một loại thiết bị ngoại vi cụ thể. Nó xử lý các giao thức truyền dữ liệu cấp thấp, đệm dữ liệu và sửa lỗi phần cứng. CPU giao tiếp với bộ điều khiển thông qua các thanh ghi trạng thái (status), điều khiển (control), và dữ liệu (data).

**So sánh thực tế**: Một cảng biển vận tải hàng hóa. Con tàu (*thiết bị ngoại vi*) cập bến tại một cầu cảng cụ thể (*cổng port*). Cần cẩu và nhân viên hải quan (*bộ điều khiển*) chịu trách nhiệm bốc dỡ hàng và kiểm tra. Mạng lưới đường lộ giao thông (*bus*) chịu trách nhiệm vận chuyển hàng hóa vào trong thành phố.

---

## Thăm dò (Polling) so với Ngắt (Interrupts)

CPU cần biết khi nào một thiết bị ngoại vi đã sẵn sàng để truyền nhận dữ liệu hoặc đã hoàn thành xong tác vụ. Hai chiến lược cơ bản:

### 1. Thăm dò (Polling)
CPU liên tục chạy vòng lặp kiểm tra thanh ghi trạng thái của thiết bị ngoại vi. Khi trạng thái báo "sẵn sàng" (ready), CPU mới thực hiện truyền dữ liệu.

```c
while ((status_register & READY_BIT) == 0) {
    // Vòng lặp chờ bận (busy wait)
}
// Thiết bị đã sẵn sàng – tiến hành truyền dữ liệu I/O
```

- **Ưu điểm**: Đơn giản, không tốn hao phí phần cứng.
- **Nhược điểm**: Lãng phí rất lớn chu kỳ xử lý của CPU (phải chạy vòng lặp chờ bận liên tục), đặc biệt là với các thiết bị ngoại vi có tốc độ phản hồi chậm.

### 2. Ngắt (Interrupts)
Thiết bị ngoại vi sẽ gửi một tín hiệu **ngắt phần cứng (interrupt)** trực tiếp lên CPU khi nó cần sự chú ý của CPU. CPU nhận tín hiệu ngắt, tạm dừng thực thi lệnh hiện tại, lưu lại trạng thái, nhảy tới chạy **trình xử lý ngắt (Interrupt Service Routine - ISR)**. Sau khi xử lý xong ngắt, CPU khôi phục lại trạng thái cũ và tiếp tục chạy tiến trình bị tạm dừng trước đó.

- **Ưu điểm**: CPU được giải phóng hoàn toàn để làm việc khác trong lúc thiết bị ngoại vi đang bận rộn xử lý.
- **Nhược điểm**: Hao phí tài nguyên để lưu/khôi phục ngữ cảnh CPU; có thể gặp hiện tượng nghẽn ngắt nếu ngắt xảy ra quá dồn dập.

**So sánh thực tế**:
- **Thăm dò**: Một đầu bếp cứ mỗi giây lại chạy đến mở lò nướng để kiểm tra xem bánh đã chín hay chưa.
- **Ngắt**: Lò nướng tự động phát tiếng bíp chuông báo (*tín hiệu ngắt*) khi bánh chín; trong lúc nướng bánh, đầu bếp có thể rảnh tay đi thái rau củ.

---

## Truy cập Bộ nhớ Trực tiếp (Direct Memory Access - DMA)

Nếu không có cơ chế DMA, CPU bắt buộc phải trực tiếp copy từng byte dữ liệu từ thiết bị ngoại vi vào RAM (phương pháp Vào/Ra lập trình sẵn - Programmed I/O - PIO), điều này làm CPU bị kẹt cứng hoàn toàn khi truyền tải các khối dữ liệu dung lượng lớn.

Bộ điều khiển **DMA (DMA Controller)** là một phần cứng chuyên dụng cho phép truyền dữ liệu trực tiếp giữa thiết bị ngoại vi và bộ nhớ RAM mà không cần sự tham gia của CPU. CPU chỉ cần cấu hình thông tin ban đầu (địa chỉ nguồn, đích, kích thước khối, hướng truyền) và tiếp tục làm việc khác. Khi DMA hoàn thành việc truyền dữ liệu, nó phát ngắt báo cho CPU biết.

**Quy trình thực hiện DMA**:
1. CPU cấu hình cho bộ điều khiển DMA: Địa chỉ nhớ RAM, cổng thiết bị, kích thước khối truyền, hướng đọc/ghi.
2. Bộ điều khiển DMA trực tiếp điều phối bus để chuyển dữ liệu (theo từng từ hoặc từng khối) qua lại giữa RAM và thiết bị.
3. Bộ điều khiển DMA phát tín hiệu ngắt báo cho CPU biết khi hoàn tất.

**So sánh thực tế**: Dịch vụ chuyển nhà. Không có DMA (PIO), bạn phải tự mình bê từng thùng đồ chất lên xe tải (*CPU bận rộn*). Có cơ chế DMA, bạn thuê một đội vận chuyển chuyên nghiệp – bạn chỉ cần chỉ cho họ: "Hãy chuyển toàn bộ đồ đạc từ phòng A sang phòng B", bạn rảnh tay làm việc khác và họ sẽ bấm chuông gọi bạn khi đã chuyển xong.

---

## Các Tầng Phần mềm Vào/Ra (I/O Software Layers)

Hệ điều hành hiện đại tổ chức hệ thống vào/ra theo dạng phân tầng, ẩn đi các chi tiết kỹ thuật phức tạp và cung cấp giao diện nhất quán cho ứng dụng.

```mermaid
flowchart TD
    UserProc[Tiến trình người dùng] --> SysCall[Giao diện lời gọi hệ thống]
    SysCall --> DevIndep[Tầng vào/ra độc lập thiết bị]
    DevIndep --> DeviceDrivers[Trình điều khiển thiết bị (Drivers)]
    DeviceDrivers --> IntHandlers[Trình xử lý ngắt]
    IntHandlers --> Hardware[Phần cứng]
```

### 1. Trình xử lý Ngắt (Interrupt Handlers)
Là tầng thấp nhất. Khi ngắt xảy ra, trình xử lý ngắt sẽ:
- Lưu các thanh ghi CPU.
- Gửi tín hiệu xác nhận ngắt với phần cứng.
- Đánh thức tiến trình đang ngủ chờ hoàn thành I/O.
- Khôi phục ngữ cảnh và trả quyền chạy cho CPU.

### 2. Trình điều khiển Thiết bị (Device Drivers)
Trình điều khiển (*driver*) là một **module nhân hệ điều hành** (kernel module) chứa mã lệnh biết cách trực tiếp giao tiếp với một thiết bị ngoại vi cụ thể. Nó dịch các lệnh chung của Hệ điều hành (ví dụ: `đọc khối đĩa số 123`) thành các thao tác ghi dữ liệu đặc thù vào các thanh ghi của bộ điều khiển phần cứng.
- Mỗi driver cài đặt một giao diện tiêu chuẩn đồng nhất (các hàm open, read, write, ioctl).
- Drivers chiếm tới 70% tổng lượng mã nguồn của các hệ điều hành hiện đại (như Linux, Windows).

### 3. Tầng Vào/Ra Độc lập Thiết bị (Device‑Independent I/O Layer)
Cung cấp các dịch vụ dùng chung thống nhất cho tất cả các loại thiết bị:
- **Đặt tên đồng nhất**: Ánh xạ thiết bị thành các đường dẫn tệp tin (ví dụ: `/dev/sda` trên Linux, ổ `C:\` trên Windows).
- **Phân quyền bảo vệ**: Kiểm soát tài khoản nào được phép truy cập thiết bị ngoại vi nào.
- **Cơ chế đệm (Buffering)**: Lưu trữ tạm dữ liệu để làm mượt luồng truyền tải dữ liệu.
- **Xử lý lỗi**: Tự động thực hiện thử lại lệnh (retry) khi gặp lỗi hoặc báo cáo lỗi cho ứng dụng.

### 4. Vào/Ra cấp Người dùng (User‑Space I/O)
Các hàm thư viện (như `printf`, `fread`) cung cấp cho lập trình viên để che giấu đi các lời gọi hệ thống (`read`, `write`) phức tạp bên dưới.

---

## Cơ chế Đệm (Buffering), Đệm ẩn (Caching), Spooling

Các kỹ thuật này giúp cải thiện hiệu năng vào/ra dữ liệu và giải quyết sự chênh lệch lớn về tốc độ giữa CPU và các thiết bị ngoại vi.

### Cơ chế Đệm (Buffering)
Một **bộ đệm** (*buffer*) là một vùng nhớ RAM dùng để lưu trữ dữ liệu tạm thời trong quá trình truyền tải dữ liệu giữa hai thực thể có tốc độ khác nhau. Bộ đệm giúp giải phóng tốc độ sản xuất dữ liệu độc lập với tốc độ tiêu thụ dữ liệu.
- **Đệm đơn (Single buffer)**: Hệ điều hành đọc dữ liệu vào một bộ đệm nhân trước, sau đó copy sang không gian nhớ của ứng dụng.
- **Đệm đôi (Double buffering)**: Sử dụng hai bộ đệm – một bộ đệm đang được nạp dữ liệu từ thiết bị ngoại vi trong khi bộ đệm còn lại đang được ứng dụng đọc xử lý.

### Đệm ẩn (Caching)
Một **bộ đệm ẩn** (*cache*) là một vùng nhớ siêu nhanh dùng để lưu trữ bản sao của dữ liệu vừa được sử dụng nhằm tăng tốc cho các lần truy cập tiếp theo.
- *Phân biệt*: **Cache** lưu trữ dữ liệu **đã sử dụng** để tái sử dụng; **Buffer** lưu trữ dữ liệu **đang trung chuyển** để điều hòa tốc độ.

### Cơ chế Spooling (Simultaneous Peripheral Operation On‑Line)
Một **spool** là một bộ đệm ổ đĩa trung gian chuyên dùng để chứa kết quả đầu ra gửi đến các thiết bị ngoại vi có tốc độ chạy siêu chậm (như máy in). Mỗi tiến trình khi muốn in sẽ ghi dữ liệu in của mình thành một tệp spool tạm thời trên đĩa cứng. Một tiến trình chạy ẩn hệ thống (daemon in ấn) sẽ lần lượt đọc các tệp này để xếp hàng in ra máy in.
- **Ưu điểm**: Các tiến trình không cần phải bị khóa block xếp hàng chờ máy in cơ học chạy xong; chúng có thể lập tức tiếp tục công việc của mình.

---

## Thiết bị dạng Khối và thiết bị dạng Ký tự

| Đặc điểm | Thiết bị dạng Khối (Block device) | Thiết bị dạng Ký tự (Character device) |
| :--- | :--- | :--- |
| **Đơn vị truy cập** | Các khối dữ liệu kích thước cố định | Luồng byte dữ liệu liên tục |
| **Cách thức truy cập** | Cho phép truy cập ngẫu nhiên (seek) | Không cho phép seek, truy cập tuần tự |
| **Các ví dụ** | Ổ cứng HDD, ổ SSD, USB drive | Bàn phím, chuột, cổng kết nối nối tiếp |
| **Giao diện gọi** | `read()`, `write()`, `seek()` | `getchar()`, `putchar()` |

---

## Cấu trúc Vật lý của Ổ đĩa cứng (Disk Structure)

Hiểu rõ cấu trúc hình học của ổ đĩa cứng HDD truyền thống giúp tối ưu hóa hiệu năng thiết kế hệ thống tệp tin:

- **Các đĩa từ (Platters)**: Các đĩa kim loại/thủy tinh tròn phủ lớp từ tính để ghi dữ liệu.
- **Trục quay (Spindle)**: Trục động cơ quay đĩa từ với tốc độ cao (5400, 7200, 10000 vòng/phút - RPM).
- **Đầu đọc/ghi (Read/write heads)**: Đầu đọc từ tính siêu nhỏ gắn trên một cần tay đỡ để di chuyển quét trên bề mặt đĩa.
- **Rãnh từ (Track)**: Các đường tròn đồng tâm phân bổ trên bề mặt đĩa từ.
- **Hình trụ (Cylinder)**: Tập hợp tất cả các rãnh từ ở cùng một vị trí đầu đọc trên toàn bộ các đĩa từ xếp chồng theo chiều dọc.
- **Phân cung (Sector)**: Đơn vị địa chỉ hóa nhỏ nhất trên đĩa từ (truyền thống là 512 byte, nay phổ biến là 4 KB).

---

## Các Cấp độ RAID (Redundant Array of Independent Disks)

**RAID** là công nghệ kết hợp nhiều ổ đĩa cứng vật lý rời rạc thành một ổ đĩa logic duy nhất nhằm tăng tốc độ truyền dữ liệu, tăng độ tin cậy an toàn dữ liệu bằng cách lưu trữ dự phòng, hoặc cả hai.

### RAID 0 (Striping - Phân mảnh đĩa)
Dữ liệu được chia nhỏ thành các khối và ghi xen kẽ song song trên các ổ đĩa. Hoàn toàn không có dữ liệu dự phòng.
- **Ưu điểm**: Tốc độ đọc/ghi cực nhanh (đọc ghi song song).
- **Hạn chế**: Chỉ cần một ổ cứng vật lý bị hỏng là mất toàn bộ dữ liệu của toàn mảng RAID.

### RAID 1 (Mirroring - So gương)
Mỗi thao tác ghi dữ liệu đều được nhân bản giống hệt nhau sang hai (hoặc nhiều hơn) ổ đĩa độc lập.
- **Ưu điểm**: An toàn dữ liệu tuyệt đối (cho phép hỏng một ổ đĩa); tốc độ đọc nhanh.
- **Hạn chế**: Chi phí lưu trữ đắt gấp đôi (chỉ tận dụng được 50% tổng dung lượng đĩa).

### RAID 5 (Striping with Parity - Ghi phân mảnh kèm mã chẵn lẻ)
Dữ liệu và mã kiểm tra chẵn lẻ (parity) được ghi phân mảnh xoay vòng trên tất cả các đĩa trong mảng. Cho phép hệ thống hoạt động bình thường ngay cả khi **hỏng một ổ đĩa bất kỳ**.
- **Ưu điểm**: Tốc độ đọc nhanh; tối ưu dung lượng lưu trữ hiệu quả hơn RAID 1 (dung lượng khả dụng là $N-1$ đĩa).
- **Hạn chế**: Tốc độ ghi bị chậm do phải tính toán mã chẵn lẻ chéo.

### RAID 6
Tương tự như RAID 5 nhưng sử dụng hai thuật toán tính mã chẵn lẻ khác nhau và ghi dự phòng kép. Cho phép hệ thống hoạt động bình thường ngay cả khi **hỏng đồng thời hai ổ đĩa vật lý**.

### RAID 10 (RAID 1+0)
Là sự kết hợp đỉnh cao – So gương dữ liệu trước (RAID 1), sau đó phân mảnh dữ liệu (RAID 0) trên các cặp đĩa so gương. Kết hợp tốc độ siêu nhanh của RAID 0 và an toàn của RAID 1.
- **Hạn chế**: Chi phí đắt đỏ (dung lượng khả dụng bằng 50% tổng dung lượng).

```mermaid
flowchart LR
    subgraph RAID0 [Mô hình RAID 0]
        D1[Khối dữ liệu 0] --> Disk1[Đĩa 1]
        D2[Khối dữ liệu 1] --> Disk2[Đĩa 2]
    end
    subgraph RAID1 [Mô hình RAID 1]
        D3[Khối dữ liệu 0] --> Disk3[Đĩa 3]
        D3 --> Disk4[Đĩa 4 (Gương)]
    end
```

---

## Bảng Tổng kết Chương

| Khái niệm | Điểm cốt lõi cần nhớ |
| :--- | :--- |
| **Phần cứng I/O** | Cổng port (đầu nối), bus hệ thống (đường truyền dẫn), bộ điều khiển controller (quản lý phần cứng). |
| **Thăm dò (Polling)** | CPU liên tục chạy vòng lặp đợi thiết bị sẵn sàng; đơn giản nhưng lãng phí chu kỳ CPU. |
| **Ngắt (Interrupt)** | Thiết bị ngoại vi phát tín hiệu báo cho CPU khi sẵn sàng; tối ưu hiệu năng cho CPU làm việc khác. |
| **Cơ chế DMA** | Phần cứng chuyển dữ liệu trực tiếp giữa thiết bị và RAM mà không làm CPU bị bận. |
| **Tầng phần mềm** | Tổ chức phân tầng: Trình xử lý ngắt → Driver thiết bị → Tầng độc lập thiết bị → Thư viện cấp người dùng. |
| **Đệm và Cache** | Bộ đệm (buffer) trung chuyển điều hòa tốc độ; Bộ đệm ẩn (cache) lưu trữ tái sử dụng dữ liệu nhanh. |
| **Khối vs. Ký tự** | Thiết bị dạng Khối (đĩa cứng, SSD) cho phép seek ngẫu nhiên; Thiết bị dạng Ký tự (chuột, phím) dạng byte stream. |
| **Các cấp độ RAID** | RAID 0 (phân mảnh tốc độ), RAID 1 (so gương an toàn), RAID 5 (mã chẵn lẻ dự phòng 1 đĩa), RAID 6 (dự phòng 2 đĩa), RAID 10 (lai tối ưu). |

Hệ thống vào/ra dữ liệu là mảnh ghép hoàn thiện bức tranh Hệ điều hành, giúp kết nối nhịp nhàng toàn bộ các cấu trúc logic nội bộ bên trong máy tính ra với các thiết bị vật lý ở thế giới thực tế ngoại vi bên ngoài.
