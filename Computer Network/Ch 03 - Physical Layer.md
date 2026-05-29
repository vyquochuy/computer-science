# Chương 3: Tầng Vật lý (Physical Layer)

## 1. Phương tiện truyền dẫn (Transmission Media)

### 1.1 Phương tiện truyền dẫn hữu tuyến (Guided Media / Wired)

```mermaid
    flowchart TD
        A["Phương tiện hữu tuyến (Guided Media)"] --> B["Cáp xoắn đôi (Twisted Pair)"]
        A --> C["Cáp đồng trục (Coaxial Cable)"]
        A --> D["Cáp quang (Optical Fiber)"]

        B --> B1["UTP (Không chống nhiễu)"]
        B --> B2["STP (Có chống nhiễu)"]
        B1 -.-> B1a["Cáp Cat5e, Cat6"]

        C --> C1["Băng tần cơ sở (Số)"]
        C --> C2["Băng thông rộng (Tương tự)"]

        D --> D1["Đơn mốt (Đường truyền xa)"]
        D --> D2["Đa mốt (Đường truyền ngắn)"]
```

### 1.2 Phương tiện truyền dẫn vô tuyến (Unguided Media / Wireless)

```mermaid
flowchart LR
    subgraph Wireless ["Truyền thông vô tuyến (Wireless)"]
        R["Sóng vô tuyến (Radio Waves)<br/>3 kHz – 1 GHz<br/>Truyền đa hướng (Omnidirectional)"]
        M["Sóng cực ngắn (Microwaves)<br/>1 GHz – 300 GHz<br/>Định hướng / Đường nhìn thẳng (Line-of-sight)"]
        I["Tia hồng ngoại (Infrared)<br/>300 GHz – 400 THz<br/>Phạm vi ngắn (Short range)"]
    end
```

---

## 2. Các dạng tín hiệu: Tương tự và Số (Analog vs Digital)

**Tín hiệu Số (Digital Signal):** Là một dạng tín hiệu rời rạc biểu diễn dữ liệu bằng các giá trị cụ thể, thông thường dưới dạng nhị phân (0 và 1). Nó thay đổi trạng thái theo các bước (rời rạc) thay vì liên tục. Tín hiệu số ít bị ảnh hưởng bởi nhiễu hơn, dễ dàng lưu trữ, xử lý, truyền tải hơn và được sử dụng rộng rãi trong máy tính và các hệ thống truyền thông hiện đại.

*Biểu diễn tín hiệu số bằng sơ đồ trạng thái (state diagram)* – mỗi trạng thái tương ứng với một mức điện áp trong một khoảng thời gian truyền bit:

```mermaid
stateDiagram-v2
    [*] --> Bit1_High : Bit 1
    Bit1_High --> Bit0_Low : Bit 0
    Bit0_Low --> Bit1_High : Bit 1
    Bit1_High --> Bit1_High2 : Bit 1
    Bit1_High2 --> Bit0_Low2 : Bit 0
    Bit0_Low2 --> [*]
    note right of Bit1_High : Điện áp cao (+V)
    note right of Bit0_Low : Điện áp thấp (-V)
```

**Tín hiệu Tương tự (Analog Signal):** Là dạng tín hiệu liên tục biến đổi mượt mà theo thời gian. Nó có thể nhận vô số giá trị trong một phạm vi nhất định. Các tín hiệu này biểu diễn các đại lượng vật lý trong thế giới thực như âm thanh, nhiệt độ và ánh sáng. Tuy nhiên, tín hiệu tương tự dễ bị nhiễu và méo mó hơn trong quá trình truyền dẫn qua khoảng cách xa.

---

## 3. Các chế độ truyền dẫn (Transmission Modes)

```mermaid
sequenceDiagram
    participant A as Thiết bị A
    participant B as Thiết bị B

    Note over A,B: Đơn công - Simplex (Chỉ truyền một chiều A → B)
    A->>B: Dữ liệu
    Note right of B: B không thể truyền ngược lại

    Note over A,B: Bán song công - Half-duplex (Một chiều tại một thời điểm)
    A->>B: Dữ liệu
    B-->>A: Xác nhận ACK (Sau khi A kết thúc)

    Note over A,B: Song công toàn phần - Full-duplex (Truyền song song đồng thời)
    A->>B: Dữ liệu
    B->>A: Dữ liệu
```

---

## 4. Kỹ thuật mã hóa đường truyền (Line Coding Techniques)

Thay vì sử dụng sơ đồ `gantt`, chúng ta sử dụng **sơ đồ trạng thái** (state diagram) để biểu thị trực quan các mức điện áp biến đổi theo thời gian.

### 4.1 Mã NRZ‑L (Non‑Return to Zero, Level)

Chuỗi bit truyền: `1 0 1 1 0 0 1`  
Quy ước điện áp: `+V` cho bit 1, `-V` cho bit 0.

```mermaid
stateDiagram-v2
    [*] --> Vplus : Bit 1 (+V)
    Vplus --> Vminus : Bit 0 (-V)
    Vminus --> Vplus : Bit 1 (+V)
    Vplus --> Vplus2 : Bit 1 (+V)
    Vplus2 --> Vminus2 : Bit 0 (-V)
    Vminus2 --> Vminus3 : Bit 0 (-V)
    Vminus3 --> Vplus3 : Bit 1 (+V)
    Vplus3 --> [*]
```

### 4.2 Mã hóa Manchester (Manchester Coding - Chuẩn IEEE 802.3)

Mỗi bit truyền luôn có một sự chuyển đổi mức điện áp ở chính giữa chu kỳ truyền bit:  
- Bit `1` = Chuyển từ mức Cao sang mức Thấp (high → low).  
- Bit `0` = Chuyển từ mức Thấp sang mức Cao (low → high).

Chuỗi bit truyền: `1 0 1 0`

```mermaid
stateDiagram-v2
    [*] --> High1 : Bit 1 bắt đầu cao
    High1 --> Low1 : Chuyển mức ở giữa (1)
    Low1 --> Low2 : Bit 0 bắt đầu thấp
    Low2 --> High2 : Chuyển mức ở giữa (0)
    High2 --> Low3 : Bit 1 bắt đầu cao
    Low3 --> High3 : Chuyển mức ở giữa (1)
    High3 --> Low4 : Bit 0 bắt đầu cao?
    Low4 --> [*]
```

*Cách diễn giải đơn giản hơn* – sử dụng bảng:

| Bit truyền | Mức điện áp lúc bắt đầu | Chuyển đổi ở giữa chu kỳ | Mức điện áp lúc kết thúc |
|-----|-------------|----------------|-----------|
| **1**   | Cao (High)  | → Thấp (Low)   | Thấp (Low) |
| **0**   | Thấp (Low)  | → Cao (High)   | Cao (High) |
| **1**   | Cao (High)  | → Thấp (Low)   | Thấp (Low) |
| **0**   | Thấp (Low)  | → Cao (High)   | Cao (High) |

### 4.3 Mã hóa lưỡng cực đảo cực AMI (Bipolar Alternate Mark Inversion)

- Bit `0` → Mức điện áp trung tính (0V).  
- Bit `1` → Đảo dấu luân phiên giữa mức điện áp cực dương (+V) và cực âm (-V).

Chuỗi bit truyền: `1 0 1 1 0 1`

```mermaid
stateDiagram-v2
    [*] --> Plus : Bit1 (+V)
    Plus --> Zero : Bit0 (0V)
    Zero --> Minus : Bit1 (-V)
    Minus --> Plus2 : Bit1 (+V)
    Plus2 --> Zero2 : Bit0 (0V)
    Zero2 --> Minus2 : Bit1 (-V)
    Minus2 --> [*]
```

---

## 5. Khái niệm tốc độ truyền dữ liệu (Data Rate Concepts)

### 5.1 Định lý định mức giới hạn Nyquist (Cho kênh truyền không có nhiễu)

Công thức tính toán:

```text
C = 2B log2(L)  | Trong đó: B = Băng thông (Bandwidth), L = Số mức tín hiệu (L), C = Dung lượng kênh truyền (Capacity)
```

### 5.2 Định luật Shannon (Cho kênh truyền có nhiễu)

Công thức tính toán:

```text
C = B log2(1 + S/N) | Trong đó: C = Dung lượng (Capacity), B = Băng thông, S/N = Tỷ số tín hiệu trên nhiễu (SNR)
```

**Sơ đồ mối quan hệ** – biểu diễn bằng sơ đồ khối:

```mermaid
flowchart TD
    subgraph NYQ ["Định lý Nyquist (Không nhiễu)"]
        N1["Tốc độ ký hiệu tối đa = 2B ký hiệu/giây"]
        N2["Với L mức: C = 2B log2(L)"]
    end
    subgraph SHN ["Khả năng Shannon (Có nhiễu)"]
        S1["Tốc độ bit tối đa dựa trên SNR"]
        S2["C = B log2(1 + S/N)"]
    end
    PRAC["Tốc độ dữ liệu thực tế <= min(Nyquist, Shannon)"]
    NYQ --> PRAC
    SHN --> PRAC
```

**Nhận thức mấu chốt:**  
- Định lý Nyquist cho biết bạn có thể gửi bao nhiêu ký hiệu (symbol) trong một giây khi không có nhiễu.  
- Định luật Shannon giới hạn thực tế số lượng bit tối đa bạn có thể mã hóa trên mỗi ký hiệu khi có nhiễu.  

**Ví dụ thực tế:** Đường dây điện thoại thông thường có băng thông $B = 3.1\text{ kHz}$, tỷ số truyền tín hiệu trên nhiễu $SNR = 30\text{ dB}$ (tương ứng tỷ số công suất $S/N = 1000$):  
- Theo Shannon: $C \approx 3100 \times \log_2(1001) \approx 30.9\text{ kbps}$.  
- Theo Nyquist khi sử dụng $L=4$ mức tín hiệu ($2\text{ bit/ký hiệu}$): $C = 2 \times 3100 \times 2 = 12.4\text{ kbps}$.  
$\rightarrow$ Kênh truyền này bị giới hạn bởi Nyquist nếu bạn chỉ sử dụng 4 mức tín hiệu. Khi có tỷ số SNR tốt hơn, bạn có thể tăng số lượng mức tín hiệu $L$, nhưng nhiễu vật lý sẽ giới hạn mức tối đa của $L$ ở giá trị $\sqrt{1 + S/N} \approx 31.6$. Khi đó tốc độ giới hạn Nyquist đạt $C \approx 2 \times 3100 \times \log_2(31.6) \approx 30.9\text{ kbps}$ – trùng với giới hạn Shannon.

---

## Bảng tổng hợp

| Khái niệm | Công thức toán học | Yếu tố giới hạn |
|---------|---------|----------------|
| **Định lý Nyquist** | $C = 2B \log_2(L)$ | Băng thông và số lượng mức tín hiệu |
| **Định luật Shannon** | $C = B \log_2(1 + S/N)$ | Băng thông và tỷ số nhiễu SNR |

---
