# Chương 5: Bế tắc (Deadlocks)

Trong môi trường đa nhiệm, các tiến trình liên tục cạnh tranh nhau để giành quyền sử dụng tài nguyên hệ thống. Đôi khi, một tập hợp các tiến trình rơi vào trạng thái bị đóng băng vĩnh viễn, trong đó mỗi tiến trình đang chờ đợi một tài nguyên được nắm giữ bởi một tiến trình khác trong tập hợp đó. Tình trạng này được gọi là **bế tắc** (*deadlock*). Chương này giải thích cách bế tắc xảy ra, cách biểu diễn chúng dưới dạng đồ thị và các chiến lược kinh điển để ngăn ngừa, phòng tránh, phát hiện hoặc bỏ qua bế tắc.

---

## Đặc trưng của Bế tắc (Deadlock Characterization)

Bế tắc xảy ra khi hai hay nhiều tiến trình cùng rơi vào trạng thái chờ đợi vô hạn một sự kiện (thường là sự kiện giải phóng tài nguyên) mà sự kiện đó chỉ có thể được tạo ra bởi một tiến trình khác cũng đang nằm trong trạng thái chờ đợi đó. Để bế tắc xảy ra, **bốn điều kiện cần** sau đây bắt buộc phải đồng thời thỏa mãn:

### Bốn Điều kiện cần của Bế tắc

| Điều kiện | Mô tả chi tiết |
| :--- | :--- |
| **Loại trừ tương hỗ (Mutual exclusion)** | Ít nhất một tài nguyên phải được giữ ở chế độ không thể chia sẻ. Tại một thời điểm, chỉ cho phép tối đa một tiến trình được phép sử dụng tài nguyên đó. |
| **Giữ và chờ (Hold and wait)** | Tiến trình đang nắm giữ ít nhất một tài nguyên và đang xếp hàng chờ đợi để được cấp thêm các tài nguyên khác đang bị nắm giữ bởi các tiến trình khác. |
| **Không trưng dụng (No preemption)** | Tài nguyên không thể bị cưỡng chế thu hồi từ tiến trình đang giữ chúng. Chỉ có tiến trình đang nắm giữ mới có quyền tự nguyện giải phóng tài nguyên. |
| **Chờ đợi vòng tròn (Circular wait)** | Tồn tại một tập hợp các tiến trình $\{P_0, P_1, \dots, P_n\}$ sao cho $P_0$ chờ tài nguyên do $P_1$ giữ, $P_1$ chờ $P_2$ giữ, ..., và $P_n$ chờ tài nguyên do $P_0$ giữ. |

**So sánh thực tế - Nút thắt giao lộ ngã tư:**  
Bốn chiếc ô tô cùng tiến vào một giao lộ ngã tư không có đèn tín hiệu và bị kẹt cứng:
- **Loại trừ tương hỗ** – Chỉ có một chiếc ô tô được phép chiếm lĩnh một khoảng diện tích giao lộ tại một thời điểm.
- **Giữ và chờ** – Mỗi xe đang giữ nguyên vị trí hiện tại của mình (đã tiến vào giao lộ) và nổ máy chờ đợi xe phía trước di chuyển.
- **Không trưng dụng** – Không có cảnh sát giao thông cẩu cưỡng chế bất kỳ xe nào đi ra chỗ khác.
- **Chờ đợi vòng tròn** – Xe A chờ xe B nhường đường; xe B chờ xe C; xe C chờ xe D; xe D lại chờ xe A. Tất cả các xe đều bị khóa cứng.

---

## Đồ thị Cấp phát Tài nguyên (Resource Allocation Graph)

**Đồ thị cấp phát tài nguyên** (Resource Allocation Graph - RAG) là đồ thị có hướng trực quan hóa trạng thái cấp phát tài nguyên của hệ thống, giúp nhanh chóng phát hiện sự tồn tại của bế tắc.

- **Tập các Đỉnh**: Chia làm hai loại chính
  - Đỉnh tiến trình (hình tròn): $P_1, P_2, \dots, P_n$.
  - Đỉnh tài nguyên (hình vuông): $R_1, R_2, \dots, R_m$ (mỗi đỉnh tài nguyên có thể chứa một hoặc nhiều đơn vị tài nguyên giống nhau).
- **Tập các Cạnh**:
  - **Cạnh yêu cầu (Request edge)** ($P_i \to R_j$): Tiến trình $P_i$ đang yêu cầu một đơn vị tài nguyên $R_j$ và đang phải xếp hàng chờ.
  - **Cạnh cấp phát (Assignment edge)** ($R_j \to P_i$): Một đơn vị tài nguyên $R_j$ đã được cấp phát và đang do tiến trình $P_i$ nắm giữ.

**Nguyên lý phát hiện bế tắc trên đồ thị RAG**:
- Nếu đồ thị **không chứa chu trình** → Hệ thống hoàn toàn không có bế tắc.
- Nếu đồ thị **chứa chu trình** và mỗi loại tài nguyên chỉ có **duy nhất một đơn vị** → Hệ thống chắc chắn đang bị bế tắc.
- Nếu đồ thị chứa chu trình nhưng các tài nguyên có **nhiều đơn vị**, chu trình chỉ biểu thị nguy cơ bế tắc tiềm ẩn chứ không bắt buộc hệ thống đang bị bế tắc (vì một số tiến trình trong chu trình có thể được giải phóng tài nguyên từ các luồng bên ngoài).

```mermaid
flowchart TD
    P1[P1] -->|yêu cầu (request)| R1((R1))
    R1 -->|được cấp phát (allocated)| P2[P2]
    P2 -->|yêu cầu (request)| R2((R2))
    R2 -->|được cấp phát (allocated)| P1
    
    style R1 fill:#f9f,stroke:#333
    style R2 fill:#f9f,stroke:#333
```

---

## Các Phương pháp Xử lý Bế tắc

Các hệ điều hành áp dụng một trong bốn phương pháp tiếp cận sau đây để đối phó với bế tắc, đi từ lý thuyết triệt để đến thực tế chấp nhận đánh đổi.

| Phương pháp | Triết lý thiết kế | Hao phí hệ thống | Tính thực tiễn |
| :--- | :--- | :--- | :--- |
| **Ngăn ngừa (Prevention)** | Thiết kế hệ thống sao cho ít nhất một trong bốn điều kiện cần không bao giờ có thể xảy ra. | Rất cao | Ít khi được áp dụng |
| **Phòng tránh (Avoidance)** | Xem xét động và cấp phát tài nguyên cực kỳ cẩn thận dựa trên các thông tin bổ sung. | Vừa phải | Khả thi cho các hệ thống tài nguyên cố định |
| **Phát hiện & Phục hồi** | Cho phép bế tắc xảy ra, định kỳ quét phát hiện và tiến hành phá vỡ bế tắc. | Vừa phải | Phổ biến trong các hệ quản trị CSDL |
| **Bỏ qua (Ignorance)** | Bỏ qua hoàn toàn, giả định bế tắc cực kỳ hiếm khi xảy ra (Thuật toán Đà điểu). | Bằng 0 | Được dùng bởi hầu hết các OS thương mại (Windows, Linux, macOS) |

---

### 1. Ngăn ngừa Bế tắc (Deadlock Prevention)

Phương pháp này triệt tiêu nguy cơ bế tắc bằng cách phá vỡ **ít nhất một** trong bốn điều kiện cần của bế tắc.

- **Phá vỡ Loại trừ tương hỗ** – Hầu như không khả thi trong thực tế vì một số tài nguyên có bản chất không thể chia sẻ (như máy in, ổ ghi đĩa). Tuy nhiên, với các tài nguyên chỉ đọc (như tệp tin read-only), ta có thể cho phép nhiều tiến trình truy cập đồng thời.
- **Phá vỡ Giữ và chờ** – Yêu cầu tiến trình phải xin cấp phát **tất cả** các tài nguyên nó cần một lần duy nhất trước khi khởi chạy, hoặc buộc tiến trình phải giải phóng toàn bộ tài nguyên hiện có trước khi gửi yêu cầu xin cấp tài nguyên mới.
  - *Nhược điểm*: Hiệu suất sử dụng tài nguyên hệ thống rất kém; dễ gây đói tài nguyên cho tiến trình lớn.
- **Phá vỡ Không trưng dụng** – Nếu một tiến trình đang nắm giữ một số tài nguyên gửi yêu cầu xin thêm tài nguyên khác mà không được đáp ứng, hệ điều hành sẽ cưỡng chế thu hồi toàn bộ tài nguyên hiện có của tiến trình đó và bắt buộc tiến trình phải xếp hàng khởi chạy lại từ đầu.
  - *Nhược điểm*: Rất khó cài đặt cho các tài nguyên có trạng thái dữ liệu phức tạp (như tệp dữ liệu đang ghi dở).
- **Phá vỡ Chờ đợi vòng tròn** – Thiết lập một **thứ tự tuyến tính toàn cục** cho tất cả các loại tài nguyên trong hệ thống (được đánh số từ $1, 2, 3 \dots$). Yêu cầu mọi tiến trình chỉ được phép xin cấp tài nguyên theo thứ tự tăng dần của các chỉ số (ví dụ: phải xin ổ băng từ (1) trước, rồi mới được xin máy in (2), và cuối cùng là đĩa cứng (3)).
  - *Tính thực tiễn*: Được áp dụng hiệu quả trong một số hệ thống nhúng hoặc các quy tắc khóa thứ tự (`lock order`) bên trong nhân Linux.

---

### 2. Phòng tránh Bế tắc (Deadlock Avoidance - Thuật toán Banker)

Thuật toán phòng tránh bế tắc hoạt động bằng cách yêu cầu mỗi tiến trình phải khai báo trước **số lượng tài nguyên tối đa** nó sẽ cần đến trong suốt quá trình chạy. Hệ điều hành sẽ giả lập cấp phát để đánh giá trạng thái an toàn của hệ thống trước khi thực sự phê duyệt yêu cầu của tiến trình.

**Khái niệm then chốt – Trạng thái An toàn (Safe State)**: Một trạng thái được gọi là an toàn nếu tồn tại ít nhất một **chuỗi an toàn** thực thi của các tiến trình sao cho mỗi tiến trình đều có thể hoàn thành công việc bằng cách sử dụng tài nguyên hiện có cộng với toàn bộ tài nguyên được giải phóng từ các tiến trình đã hoàn thành trước đó.

Hệ điều hành sẽ **từ chối hoặc trì hoãn** yêu cầu cấp phát của tiến trình nếu việc phê duyệt yêu cầu đó đưa hệ thống vào **trạng thái không an toàn (unsafe state)**.

**Các cấu trúc dữ liệu chính trong Thuật toán Banker** (cho $n$ tiến trình và $m$ loại tài nguyên):
- `Available[m]`: Số lượng tài nguyên có sẵn của từng loại.
- `Max[n][m]`: Số lượng tài nguyên yêu cầu tối đa của mỗi tiến trình.
- `Allocation[n][m]`: Số lượng tài nguyên hiện đang cấp phát cho từng tiến trình.
- `Need[n][m]` = `Max[i][j] - Allocation[i][j]`: Số lượng tài nguyên còn cần thêm của từng tiến trình.

#### Thuật toán Kiểm tra Trạng thái An toàn (Safety Algorithm)
1. Khởi tạo mảng `Work = Available` và mảng trạng thái `Finish[i] = false` cho mọi $i \in [0, n-1]$.
2. Tìm một tiến trình $i$ thỏa mãn đồng thời:
   - `Finish[i] == false`
   - `Need[i] <= Work`
   Nếu không tìm thấy tiến trình nào thỏa mãn, nhảy sang bước 4.
3. Cập nhật `Work = Work + Allocation[i]` và đặt `Finish[i] = true`. Quay lại bước 2.
4. Nếu `Finish[i] == true` cho tất cả mọi $i$, hệ thống ở trạng thái **An toàn**.

#### Thuật toán Cấp phát Tài nguyên (Resource‑Request Algorithm)
Khi tiến trình $P_i$ gửi yêu cầu cấp phát tài nguyên `Request[i]`:
1. Nếu `Request[i] > Need[i]`: Báo lỗi (tiến trình yêu cầu vượt quá khai báo tối đa).
2. Nếu `Request[i] > Available`: Tiến trình $P_i$ phải đi ngủ chờ đợi (tài nguyên hiện chưa đủ cấp).
3. Giả lập cấp phát tài nguyên bằng cách cập nhật các biến:
   - `Available = Available - Request[i]`
   - `Allocation[i] = Allocation[i] + Request[i]`
   - `Need[i] = Need[i] - Request[i]`
4. Chạy Thuật toán Kiểm tra Trạng thái An toàn.
   - Nếu kết quả là **An toàn** → Phê duyệt cấp phát tài nguyên thực tế cho tiến trình.
   - Nếu kết quả là **Không an toàn** → Khôi phục lại trạng thái dữ liệu cũ (rollback) và bắt buộc tiến trình $P_i$ phải chờ đợi.

*Nhược điểm của thuật toán*: Đòi hỏi số lượng tiến trình và tài nguyên phải cố định; tiến trình bắt buộc phải biết trước nhu cầu tối đa của mình (điều này hầu như không thể trong các hệ điều hành đa dụng); độ phức tạp tính toán khá cao $O(m \times n^2)$ cho mỗi yêu cầu cấp phát.

---

### 3. Phát hiện và Phục hồi Bế tắc

Phương pháp này chấp nhận cho bế tắc xảy ra, định kỳ chạy chương trình phát hiện bế tắc và tiến hành các biện pháp phá vỡ bế tắc.

- **Phát hiện bế tắc trên tài nguyên đơn vị (Single Instance)**: Sử dụng **Đồ thị Chờ đợi (Wait‑for Graph)** – Là phiên bản rút gọn từ đồ thị RAG bằng cách lược bỏ các đỉnh tài nguyên; một cạnh có hướng $P_i \to P_j$ biểu thị tiến trình $P_i$ đang chờ tài nguyên do tiến trình $P_j$ nắm giữ. Sử dụng các thuật toán tìm chu trình (như duyệt theo chiều sâu DFS) định kỳ để phát hiện bế tắc.
- **Phát hiện bế tắc trên tài nguyên đa đơn vị**: Áp dụng thuật toán tương tự như kiểm tra tính an toàn của thuật toán Banker (không cần khai báo `Max`) để tìm xem có tiến trình nào không thể hoàn thành công việc hay không.

**Các phương án Phục hồi sau bế tắc**:

| Giải pháp | Mô tả chi tiết | Hạn chế |
| :--- | :--- | :--- |
| **Hủy bỏ tiến trình** | Buộc dừng (kill) toàn bộ các tiến trình bị bế tắc, hoặc hủy từng tiến trình một cho đến khi hết chu trình bế tắc. | Chi phí rất đắt; làm mất toàn bộ kết quả công việc đang chạy dở của tiến trình. |
| **Trưng dụng tài nguyên** | Cưỡng chế thu hồi tài nguyên từ một tiến trình bị bế tắc để cấp phát cho tiến trình khác. | Phải thiết lập điểm khôi phục (checkpoint) để rollback tiến trình về trạng thái an toàn trước đó; dễ gây đói tài nguyên cho tiến trình bị thu hồi nhiều lần. |

---

### 4. Bỏ qua Bế tắc (Thuật toán Đà điểu - Ostrich Algorithm)

Thuật toán Đà điểu giải quyết bế tắc bằng cách... coi như không biết và bỏ qua hoàn toàn vấn đề, tương tự như việc chú đà điểu giấu đầu vào cát khi gặp nguy hiểm. Phương pháp này dựa trên giả định thực tế rằng bế tắc xảy ra cực kỳ hiếm hoi và chi phí để duy trì các giải pháp phòng tránh, ngăn ngừa hay phát hiện liên tục là quá đắt đỏ so với thiệt hại.

- Được sử dụng trong **hầu hết các hệ điều hành đa dụng hiện đại** (Windows, Linux, macOS).
- **Lý do**: Bế tắc phần lớn xảy ra do lỗi logic (bug) của lập trình viên khi thiết kế mã nguồn đa luồng, không phải do thiết kế của Hệ điều hành. Hệ điều hành cung cấp đầy đủ công cụ đồng bộ hóa, việc sử dụng sao cho không bị chờ đợi vòng tròn là trách nhiệm của lập trình viên.
- Nếu bế tắc xảy ra làm treo một số chương trình, người dùng có thể tắt chương trình đó đi hoặc khởi động lại máy tính.

---

## Bảng Tổng kết Chương

| Khái niệm | Điểm cốt lõi cần nhớ |
| :--- | :--- |
| **Bốn điều kiện cần** | Loại trừ tương hỗ, giữ và chờ, không trưng dụng, chờ đợi vòng tròn (thiếu 1 trong 4 thì bế tắc không thể xảy ra). |
| **Đồ thị RAG** | Công cụ đồ thị biểu diễn tiến trình, tài nguyên và các cạnh yêu cầu/cấp phát để phát hiện chu trình. |
| **Ngăn ngừa bế tắc** | Thiết kế phá vỡ 1 trong 4 điều kiện cần (phổ biến nhất là thiết lập thứ tự tuyến tính xin cấp tài nguyên). |
| **Phòng tránh bế tắc** | Sử dụng thuật toán Banker để kiểm tra và giữ hệ thống luôn ở trạng thái an toàn (safe state). |
| **Phát hiện & Phục hồi** | Định kỳ quét chu trình (bằng đồ thị Wait-for); phục hồi bằng cách kill tiến trình hoặc thu hồi tài nguyên (preempt). |
| **Bỏ qua bế tắc** | Triết lý thực dụng của Windows/Linux; bỏ qua bế tắc để tối ưu hiệu năng; người dùng tự xử lý bằng cách reset. |

Hiểu rõ về bế tắc giúp bạn thiết kế và viết các chương trình đa luồng an toàn, tối ưu hiệu năng. Chương tiếp theo sẽ dẫn chúng ta chuyển sang tìm hiểu về thế giới phần cứng vật lý cốt lõi của Hệ điều hành: **Quản lý Bộ nhớ (Memory Management)**.
