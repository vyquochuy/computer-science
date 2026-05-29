# Chương 10: An ninh và Bảo vệ (Security and Protection)

An ninh và bảo vệ là những yêu cầu nền tảng tối quan trọng của bất kỳ hệ điều hành nào. Chương này giới thiệu các mục tiêu an ninh cốt lõi, phương pháp xác thực người dùng, các mô hình kiểm soát truy cập, cơ chế bảo vệ trong nhân, các loại mã độc phổ biến, các hình thức tấn công tràn bộ đệm hệ thống và các kỹ thuật mã hóa dùng để bảo vệ an toàn cho dữ liệu cũng như tài nguyên của máy tính.

---

## Các Mục tiêu An ninh: Bảo mật, Toàn vẹn, Sẵn sàng

**Bộ ba CIA (CIA triad)** định nghĩa các mục tiêu cốt lõi của an ninh thông tin.

| Mục tiêu | Ý nghĩa kỹ thuật | Ví dụ về sự cố thất bại |
| :--- | :--- | :--- |
| **Tính bảo mật (Confidentiality)** | Ngăn chặn việc truy cập thông tin trái phép từ những người không có quyền. | Kẻ tấn công đánh cắp thành công tệp lưu mật khẩu hệ thống. |
| **Tính toàn vẹn (Integrity)** | Đảm bảo dữ liệu không bị thay đổi, xóa bỏ hoặc sửa đổi trái phép. | Mã độc xâm nhập và âm thầm sửa đổi các tệp lệnh hệ thống. |
| **Tính sẵn sàng (Availability)** | Đảm bảo hệ thống và dữ liệu luôn có thể truy cập được khi người dùng cần. | Tấn công từ chối dịch vụ (DoS) làm nghẽn khiến website bị sập. |

**So sánh thực tế - Hầm chứa tiền của ngân hàng:**
- **Tính bảo mật**: Chỉ nhân viên ngân hàng có thẩm quyền mới được phép bước vào xem và kiểm đếm tiền.
- **Tính toàn vẹn**: Tiền lưu trữ không thể bị bớt xén hay thay thế bằng tiền giả (ngăn ngừa tiền giả).
- **Tính sẵn sàng**: Khách hàng luôn có thể rút được tiền trong khung giờ làm việc của ngân hàng.

---

## Xác thực Người dùng (Authentication)

**Xác thực** (*Authentication*) là quá trình xác minh danh tính thực tế của người dùng hoặc tiến trình trước khi cho phép gia nhập hệ thống. Các yếu tố xác thực phổ biến:

### 1. Yếu tố hiểu biết – Những gì bạn biết (Mật khẩu, mã PIN)
Là phương thức phổ biến nhất, nhưng dễ bị tấn công đoán mò, đánh cắp vật lý, hoặc lừa đảo (phishing).
- **Lưu trữ mật khẩu**: Các hệ điều hành hiện đại không bao giờ lưu mật khẩu dạng văn bản gốc (plaintext) trên đĩa cứng. Chúng lưu mật khẩu dưới dạng **bản băm (hash)** thông qua các thuật toán băm an toàn như bcrypt, SHA-256 kết hợp với muối (*salt*).
- **Muối (Salt)**: Là một chuỗi ký tự ngẫu nhiên được tự động thêm vào trước mật khẩu trước khi băm, giúp triệt tiêu hoàn toàn hiệu quả của kiểu tấn công tra bảng băm dựng sẵn (rainbow table).

### 2. Yếu tố sở hữu – Những gì bạn có (Thẻ thông minh, khóa bảo mật, điện thoại)
Cung cấp yếu tố xác thực thứ hai; thường được kết hợp với mật khẩu để tạo nên cơ chế **xác thực hai yếu tố (2FA)**.
- Khóa bảo mật phần cứng chuyên dụng: YubiKey, RSA SecurID.
- Khóa phần mềm: Cơ chế mã OTP theo thời gian (TOTP - ví dụ: Google Authenticator).

### 3. Yếu tố sinh trắc – Những gì là chính bạn (Biometrics)
Dấu vân tay, quét mống mắt, nhận diện khuôn mặt, giọng nói.
- **Ưu điểm**: Thuận tiện cực cao, rất khó làm giả vật lý.
- **Hạn chế**: Không thể thay đổi hoặc cấp lại nếu thông tin sinh trắc học bị rò rỉ; có thể xảy ra tỷ lệ nhận diện sai.

### Xác thực đa yếu tố (Multi‑factor Authentication - MFA)
Kết hợp đồng thời từ hai yếu tố xác thực trở lên. Ví dụ: Yêu cầu điền mật khẩu (*yếu tố hiểu biết*) + nhập mã OTP gửi về điện thoại (*yếu tố sở hữu*). Giúp nâng cao vượt trội độ an toàn cho hệ thống.

---

## Kiểm soát Truy cập (Access Control)

Sau khi xác thực danh tính thành công, Hệ điều hành phải đưa ra quyết định người dùng được phép truy cập tài nguyên nào và thực hiện các quyền thao tác gì. Ba mô hình kiểm soát truy cập kinh điển:

### 1. DAC (Discretionary Access Control - Kiểm soát tùy quyền)
Quyền truy cập tài nguyên được quyết định hoàn toàn bởi **chủ sở hữu (owner)** của tài nguyên đó. Đây là mô hình phổ biến nhất trên các hệ điều hành đa dụng (như phân quyền tệp tin trên UNIX, danh sách quyền ACLs trên Windows).
- *Ví dụ*: Phân quyền Read, Write, Execute cho Owner, Group, Others trên hệ điều hành Linux.
- *Nhược điểm*: Người dùng có thể vô tình cấu hình phân quyền quá rộng rãi gây sơ hở an ninh.

### 2. MAC (Mandatory Access Control - Kiểm soát bắt buộc)
Quyền truy cập được quyết định và áp đặt cứng bởi **hệ thống** (Hệ điều hành) dựa trên chính sách bảo mật toàn cục, người dùng không thể tự ý thay đổi. Thường được áp dụng trong các môi trường yêu cầu an ninh cực cao (quân đội, chính phủ, các hệ thống tăng cường SELinux, AppArmor).
- **Nhãn an ninh (Labels)**: Mỗi tài nguyên và người dùng được gán một nhãn phân cấp bảo mật (ví dụ: "Tối mật", "Mật", "Nội bộ", "Công cộng").
- **Quy tắc**: Người dùng chỉ được truy cập tài nguyên nếu cấp độ nhãn an ninh của họ cao hơn hoặc bằng cấp độ nhãn của tài nguyên đó.

### 3. RBAC (Role‑Based Access Control - Kiểm soát theo vai trò)
Phân quyền truy cập được gán cho các **vai trò (roles)** cụ thể, và sau đó tài khoản người dùng sẽ được gán vào các vai trò tương ứng (ví dụ vai trò: "Bác sĩ", "Y tá", "Kế toán").
- **Ưu điểm**: Đơn giản hóa tối đa công tác quản trị hệ thống – khi nhân sự thay đổi, chỉ cần thay đổi phân quyền của vai trò hoặc gán vai trò mới cho nhân sự đó.
- *Ví dụ*: Các nhóm người dùng (groups) trên hệ điều hành Windows, hệ quản trị cơ sở dữ liệu.

---

## Cơ chế Bảo vệ: Capabilities và Ma trận Truy cập

Hệ điều hành sử dụng các cấu trúc dữ liệu nội bộ để thực thi việc kiểm soát truy cập.

### Ma trận Truy cập (Access Matrix)
Là một bảng khái niệm trong đó các dòng đại diện cho **chủ thể** (subjects - người dùng, tiến trình) và các cột đại diện cho **đối tượng** (objects - tệp tin, thiết bị). Mỗi ô trong bảng chứa các **quyền thao tác** được phép thực hiện (đọc, ghi, thực thi, xóa).

| Chủ thể | Tệp A | Tệp B | Máy in |
| :--- | :--- | :--- | :--- |
| **Alice** | đọc, ghi | đọc | (không có) |
| **Bob** | đọc | đọc, ghi | in ấn |

**Phương án hiện thực hóa trong Hệ điều hành**:
- **Danh sách Kiểm soát Truy cập (Access Control Lists - ACLs)**: Đóng gói và gắn danh sách quyền trực tiếp vào từng **đối tượng** (tương ứng với việc quản lý theo từng cột của ma trận). Được dùng trên hệ thống tệp Windows NTFS, POSIX ACLs trên Linux.
- **Danh sách Năng lực (Capabilities)**: Đóng gói và gắn danh sách quyền trực tiếp vào từng **chủ thể** (tương ứng với việc quản lý theo từng dòng của ma trận). Mỗi năng lực là một token đặc quyền không thể làm giả do nhân hệ điều hành cấp phát và quản lý.

---

## Nguyên tắc Đặc quyền Tối thiểu (Principle of Least Privilege)

Nguyên tắc **đặc quyền tối thiểu** quy định rằng một tiến trình (hoặc người dùng) chỉ được cấp phát các quyền hạn tối thiểu vừa đủ để hoàn thành công việc của mình, và chỉ được giữ các quyền hạn đó trong khoảng thời gian ngắn nhất cần thiết.

- **Lợi ích**: Giới hạn tối đa phạm vi ảnh hưởng và thiệt hại nếu chương trình bị dính lỗi hoặc bị hacker tấn công chiếm quyền điều khiển.
- **Ví dụ thực tế**:
  - Các chương trình chạy quyền `setuid` trên UNIX sẽ tự động hạ đặc quyền xuống cấp người dùng bình thường ngay sau khi hoàn tất các cấu hình đặc quyền ban đầu.
  - Các tab của trình duyệt web hiện đại chạy trong các môi trường cô lập (sandboxes) bị giới hạn nghiêm ngặt quyền truy cập ổ đĩa cứng.
  - Phân rã quyền quản trị gốc `root` trên Linux thành nhiều mảnh năng lực nhỏ độc lập (như `CAP_NET_ADMIN` chuyên quản lý mạng, `CAP_SYS_ADMIN` chuyên quản trị hệ thống).

**So sánh thực tế**: Chìa khóa dịch vụ của khách sạn – nhân viên đỗ xe chỉ có quyền lái xe của bạn vào bãi đỗ nhưng hoàn toàn không có quyền mở két sắt bên trong xe của bạn.

---

## Phần mềm Độc hại (Malware)

Mã độc là các phần mềm được thiết kế có chủ đích nhằm phá hoại, xâm nhập trái phép hoặc đánh cắp dữ liệu của hệ thống.

| Loại mã độc | Đặc điểm nhận diện | Cơ chế lan truyền | Hậu quả gây ra |
| :--- | :--- | :--- | :--- |
| **Virus** | Gắn chặt mã độc vào các chương trình hợp lệ khác; bắt buộc phải có hành động chạy tệp nhiễm của người dùng mới có thể kích hoạt. | Lây nhiễm file | Hủy hoại dữ liệu, tạo cửa sau (backdoor). |
| **Sâu máy tính (Worm)** | Chương trình độc lập có khả năng tự động nhân bản và lây lan trực tiếp qua mạng Internet mà không cần hành động của người dùng. | Quét lỗi mạng, email | Làm cạn kiệt tài nguyên mạng và hệ thống. |
| **Trojan** | Ngụy trang dưới dạng một phần mềm hợp lệ, hữu ích; không có khả năng tự nhân bản. | Đánh lừa người dùng tải về | Đánh cắp dữ liệu tài khoản, cài ransomware. |
| **Rootkit** | Mã độc cấp sâu được thiết kế để ẩn giấu sự hiện diện của chính nó (ẩn file, tiến trình, kết nối mạng) trước các công cụ quét của OS. | Cài đặt sau khi đã chiếm được quyền root/admin | Kiểm soát hệ thống lâu dài và tàng hình. |
| **Ransomware (Mã độc tống tiền)** | Tiến hành mã hóa toàn bộ các tệp dữ liệu của người dùng và đòi tiền chuộc để giải mã. | Qua Trojan, Worm, lỗi bảo mật | Khóa chặt dữ liệu người dùng. |

---

## Tấn công Tràn Bộ đệm và các Cơ chế Phòng vệ

**Tràn bộ đệm (Buffer overflow)** xảy ra khi một chương trình thực hiện ghi dữ liệu vượt quá biên giới hạn của bộ đệm được cấp phát, làm ghi đè lên các vùng dữ liệu kề cận trong bộ nhớ. Kẻ tấn công lợi dụng sơ hở này để thay đổi luồng thực thi lệnh của chương trình hoặc chèn mã độc vào hệ thống.

### Lỗi tràn stack kinh điển
```c
void vulnerable(char *str) {
    char buffer[64];
    strcpy(buffer, str);   // Lỗi nghiêm trọng: Không kiểm tra biên giới hạn của str
}
```
Nếu chuỗi đầu vào `str` dài hơn 64 byte, nó sẽ ghi đè lên địa chỉ trả về (*return address*) của hàm nằm trong stack. Kẻ tấn công có thể chèn mã độc (*shellcode*) vào đệm và ghi đè địa chỉ trả về trỏ đúng vào vùng mã độc đó để CPU thực thi.

### Các Cơ chế Phòng vệ Tích hợp trong OS

- **Phân đoạn ngăn xếp không cho phép thực thi (NX bit / DEP)**: Đánh dấu vùng nhớ ngăn xếp (stack) và heap chỉ có quyền đọc/ghi, không có quyền thực thi lệnh. Ngăn chặn tuyệt đối việc CPU chạy mã độc shellcode chèn trên stack.
- **Ngẫu nhiên hóa sơ đồ không gian địa chỉ (ASLR)**: Hệ điều hành tự động thay đổi ngẫu nhiên địa chỉ cơ sở của vùng stack, heap và các thư viện hệ thống mỗi khi chương trình khởi chạy. Kẻ tấn công không thể dự đoán chính xác địa chỉ bộ nhớ để nhảy lệnh đến.
- **Chim hoàng yến ngăn xếp (Stack Canaries)**: Trình biên dịch đặt một giá trị ngẫu nhiên bí mật (*canary*) nằm ngay trước địa chỉ trả về của hàm trong stack. Trước khi hàm thoát ra và nhảy lệnh, hệ thống sẽ kiểm tra xem giá trị canary này có bị thay đổi không; nếu bị thay đổi (biểu thị có tràn bộ đệm), chương trình sẽ lập tức báo động và tự động chấm dứt để ngăn chặn tấn công.
- **Kiểm tra biên giới hạn**: Thực hiện kiểm tra biên lúc runtime hoặc sử dụng các ngôn ngữ an toàn bộ nhớ (như Rust, Go).

---

## Cơ chế Mã hóa bảo vệ Hệ điều hành

Mã hóa biến đổi dữ liệu thành dạng không thể đọc được nếu không có khóa giải mã hợp lệ, giúp bảo vệ dữ liệu ở cả hai trạng thái: Lưu trữ trên đĩa cứng (*data at rest*) và truyền tải trên mạng (*data in transit*).

### Mã hóa toàn bộ đĩa cứng (Full‑Disk Encryption - FDE)
Mã hóa toàn bộ phân vùng lưu trữ của thiết bị (ngoại trừ khối khởi động boot block). Hệ điều hành thực hiện giải mã trực tiếp lúc runtime sử dụng khóa được giải mã từ mật khẩu người dùng hoặc chip bảo mật **TPM (Trusted Platform Module)** trên bo mạch chủ.
- **Các công nghệ FDE phổ biến**: Windows BitLocker, macOS FileVault, Linux LUKS (dm-crypt).
- **Mô hình đe dọa**: Chỉ bảo vệ an toàn dữ liệu tránh bị mất cắp khi máy tính bị mất trộm vật lý; hoàn toàn không bảo vệ được nếu máy tính đang chạy và đã được đăng nhập hợp lệ.

---

## Bảng Tổng kết Chương

| Khái niệm | Điểm cốt lõi cần nhớ |
| :--- | :--- |
| **Mục tiêu an ninh** | Bộ ba CIA: tính bảo mật (confidentiality), tính toàn vẹn (integrity), tính sẵn sàng (availability). |
| **Xác thực** | Xác minh danh tính qua kiến thức (mật khẩu), sở hữu (token), sinh trắc; kết hợp thành xác thực đa yếu tố (MFA). |
| **Kiểm soát truy cập** | Mô hình DAC (chủ sở hữu quyết định), MAC (hệ thống áp đặt nhãn), RBAC (phân quyền theo vai trò). |
| **Cơ chế bảo vệ** | Quản lý bằng ma trận truy cập; triển khai dạng danh sách ACL hoặc danh sách năng lực (capabilities). |
| **Tràn bộ đệm** | Sơ hở ghi đè địa chỉ trả về trên stack; phòng vệ bằng các cơ chế NX bit, ASLR, và chim hoàng yến (canaries). |
| **Mã hóa đĩa FDE** | BitLocker/FileVault/LUKS mã hóa toàn bộ đĩa từ để chống lại nguy cơ mất trộm phần cứng vật lý. |

An ninh mạng không phải là một đích đến tĩnh mà là một quy trình cải tiến liên tục. Việc nắm vững các cơ chế bảo vệ cấp hệ thống này là viên gạch đầu tiên giúp bạn xây dựng và duy trì các hệ thống phần mềm tin cậy, an toàn.
