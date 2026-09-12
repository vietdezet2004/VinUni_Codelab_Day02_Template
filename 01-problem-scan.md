# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

# Phase 1 — SCAN

## List bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Xanh SM | Stakeholder Pain | Khách hàng gặp vấn đề về pin xe điện nhưng khó tìm được trạm sạc phù hợp trong bán kính mong muốn; cần nhập vị trí GPS và bán kính để hệ thống tìm trạm sạc gần nhất. |
| 2 | VinFast | Repetitive | Nhân viên phải kiểm tra và đối soát thủ công dữ liệu sạc của xe điện giữa hệ thống xe, trạm sạc và hóa đơn, gây mất thời gian khi xử lý số lượng lớn giao dịch. |
| 3 | Vinhomes | Time-consuming | Nhân viên CSKH mất nhiều thời gian đọc và phân loại phản ánh của cư dân trước khi chuyển ticket đến đúng bộ phận xử lý. |
| 4 | Vinmec | Time-consuming | Nhân viên phải tìm kiếm và tổng hợp thông tin từ nhiều hồ sơ y tế khi chuẩn bị dữ liệu hỗ trợ bác sĩ xem xét lịch sử khám của bệnh nhân. |
| 5 | Vinpearl / VinWonders | AI-upgrade | Khách du lịch phải tự tìm kiếm thông tin về địa điểm, thời gian hoạt động và dịch vụ trong khu vui chơi; hệ thống có thể hỗ trợ trả lời câu hỏi và đề xuất lịch trình phù hợp. |

### Phân tích nhanh

**Bài toán #1 — Xanh SM: Tìm trạm sạc gần nhất**

Đây là bài toán có workflow tương đối rõ ràng và có thể đo lường được. Input chính gồm vị trí GPS của khách hàng và bán kính tìm kiếm. Hệ thống có thể truy vấn dữ liệu các trạm sạc, tính khoảng cách và trả về trạm phù hợp.

Bài toán này không nhất thiết phải sử dụng LLM cho phần tính toán khoảng cách. Rule-based/geospatial search có thể xử lý phần tìm kiếm chính xác và nhanh hơn. AI/LLM chỉ nên được sử dụng ở lớp giao tiếp tự nhiên nếu khách hàng nhập yêu cầu bằng ngôn ngữ tự nhiên.

Ví dụ:

> "Tôi đang ở vị trí GPS này, tìm cho tôi trạm sạc trong bán kính 5 km."

Hệ thống có thể chuyển query thành:

```text
latitude = 21.0285
longitude = 105.8542
radius = 5 km
```

Sau đó backend thực hiện tìm kiếm các trạm nằm trong bán kính và trả về trạm gần nhất.

---

# Phase 2 — QUICK-ASSESS

## QUICK PROBLEM CARD #1 — Xanh SM Intelligent Charging Station Search

### Bài toán (1 câu)

Khách hàng cung cấp vị trí GPS và bán kính tìm kiếm, nhưng việc xác định trạm sạc phù hợp và gần nhất có thể gây mất thời gian; cần một hệ thống tự động tìm và trả về trạm sạc gần nhất trong phạm vi cho phép.

### Công ty thành viên

- [ ] VinFast
- [x] Xanh SM
- [ ] Vinhomes
- [ ] Vinmec
- [ ] Khác

### Ai đang đau (Actor)?

- Khách hàng Xanh SM sử dụng xe điện.
- Tài xế cần tìm trạm sạc khi mức pin thấp.
- Nhân viên hỗ trợ/CSKH khi phải hướng dẫn khách hàng tìm trạm sạc.

### Workflow thủ công hiện tại

```text
1. Khách hàng xác định vị trí hiện tại
        ↓
2. Khách hàng nhập/tìm vị trí trên ứng dụng
        ↓
3. Khách hàng chọn bán kính tìm kiếm
        ↓
4. Hệ thống tìm các trạm sạc trong khu vực
        ↓
5. Tính khoảng cách từ vị trí khách hàng đến từng trạm
        ↓
6. Sắp xếp các trạm theo khoảng cách
        ↓
7. Trả về trạm sạc gần nhất
```

### Bước nào tốn thời gian/lỗi nhất?

Bước **3 → 6**, đặc biệt khi người dùng nhập query bằng ngôn ngữ tự nhiên hoặc thông tin GPS/bán kính không đúng định dạng.

Ví dụ:

> "Tôi đang ở đây, tìm trạm sạc trong vòng 5km."

Hệ thống phải xác định chính xác:

```text
GPS = latitude + longitude
Radius = 5 km
```

Sau đó mới thực hiện truy vấn dữ liệu địa lý.

**Thời gian mục tiêu:**

- Quy trình thủ công/hỗ trợ: khoảng **3–5 phút/lượt**
- Sau khi tự động hóa: mục tiêu **dưới 10 giây/lượt**

> Các con số trên là giả định dùng để xây dựng prototype và cần được xác thực bằng dữ liệu vận hành thực tế.

### AI có thể nhảy vào hỗ trợ ở bước nào?

AI có thể hỗ trợ ở bước **3**, khi chuyển query tự nhiên của khách hàng thành các tham số có cấu trúc.

Ví dụ:

```text
User Query:
"Tìm cho tôi trạm sạc gần nhất trong bán kính 5 km."

        ↓

LLM / Query Parser

        ↓

{
    "radius_km": 5,
    "search_type": "nearest_charging_station"
}

        ↓

Geospatial Search Engine

        ↓

Charging Station Database

        ↓

Nearest Station
```

Tuy nhiên, **LLM không nên trực tiếp tính khoảng cách hoặc tự quyết định trạm sạc**.

Phần tìm kiếm và tính khoảng cách nên được xử lý bằng code/geospatial database để đảm bảo tính chính xác.

### Đo thành công bằng gì?

- Thời gian xử lý mỗi query: **3–5 phút → dưới 10 giây**
- Tỷ lệ query trả về ít nhất một trạm phù hợp: **> 95%**
- Tỷ lệ xác định đúng GPS và bán kính từ query: **> 98%**
- Tỷ lệ chọn đúng trạm gần nhất trong phạm vi yêu cầu: **> 99%**
- Giảm số lượng yêu cầu khách hàng phải liên hệ CSKH để tìm trạm sạc: **20–30%**

### Quick Architecture

- [ ] No AI
- [x] Rule
- [x] LLM
- [ ] Agent

### Kiến trúc đề xuất

```text
Customer Query
      ↓
LLM / Structured Query Parser
      ↓
Validation / Guardrail
      ↓
Geospatial Search
      ↓
Charging Station Database
      ↓
Calculate Distance
      ↓
Nearest Suitable Station
      ↓
Response to Customer
```

### Ranh giới vận hành

LLM chỉ được phép:

1. Hiểu query của khách hàng.
2. Trích xuất GPS/location nếu có.
3. Trích xuất bán kính tìm kiếm.
4. Chuyển yêu cầu thành structured parameters.

LLM **không được phép**:

1. Tự bịa tọa độ GPS.
2. Tự bịa tên hoặc vị trí trạm sạc.
3. Tự tính khoảng cách thay cho hệ thống địa lý.
4. Khẳng định một trạm đang hoạt động nếu database chưa xác nhận.
5. Tự thực hiện hành động ngoài phạm vi tìm kiếm.

Nếu thiếu GPS hoặc bán kính:

```text
Ask clarification
```

Nếu không có trạm trong bán kính:

```text
Không tìm thấy trạm phù hợp trong bán kính yêu cầu.
Có thể đề xuất mở rộng bán kính nếu người dùng đồng ý.
```

---

# QUICK PROBLEM CARD #2 — Vinhomes Resident Complaint Classification

### Bài toán (1 câu)

Nhân viên CSKH Vinhomes phải đọc và phân loại thủ công phản ánh của cư dân trước khi chuyển đến bộ phận xử lý phù hợp, gây tốn thời gian và có thể phân loại sai.

### Công ty thành viên

- [ ] VinFast
- [ ] Xanh SM
- [x] Vinhomes
- [ ] Vinmec
- [ ] Khác

### Ai đang đau (Actor)?

- Nhân viên CSKH.
- Bộ phận vận hành tòa nhà.
- Cư dân chờ phản hồi.

### Workflow thủ công hiện tại

```text
1. Cư dân gửi phản ánh
        ↓
2. Nhân viên đọc nội dung
        ↓
3. Xác định loại vấn đề
        ↓
4. Xác định mức độ ưu tiên
        ↓
5. Chuyển ticket đến bộ phận phụ trách
```

### Bước nào tốn thời gian/lỗi nhất?

**Bước 2 → 4**, đặc biệt khi số lượng phản ánh lớn hoặc nội dung không theo mẫu.

Ước tính:

```text
5–10 phút/ticket
        ↓
Mục tiêu: dưới 1 phút/ticket
```

### AI có thể nhảy vào hỗ trợ ở bước nào?

LLM có thể đọc nội dung phản ánh và trích xuất:

```json
{
  "category": "elevator",
  "priority": "high",
  "location": "Tower A",
  "summary": "Thang máy không hoạt động"
}
```

Sau đó hệ thống Rule-based quyết định routing ticket.

### Đo thành công bằng gì?

- Thời gian phân loại: **5–10 phút → dưới 1 phút**
- Classification accuracy: **> 90%**
- Giảm ticket routing sai: **30%**
- Giảm thời gian phản hồi ban đầu: **50%**

### Quick Architecture

- [ ] No AI
- [x] Rule
- [x] LLM
- [ ] Agent

### Ranh giới

LLM chỉ phân loại và tóm tắt.

LLM không tự đóng ticket, không tự xác nhận vấn đề đã được giải quyết và không tự thực hiện hành động vận hành.

---

# QUICK PROBLEM CARD #3 — Vinpearl / VinWonders AI Guest Assistant

### Bài toán (1 câu)

Khách hàng phải tự tìm kiếm thông tin về địa điểm, dịch vụ, thời gian hoạt động và các tiện ích trong khu du lịch, dẫn đến nhiều câu hỏi lặp lại cho nhân viên CSKH.

### Công ty thành viên

- [ ] VinFast
- [ ] Xanh SM
- [ ] Vinhomes
- [ ] Vinmec
- [x] Khác: Vinpearl / VinWonders

### Ai đang đau (Actor)?

- Khách du lịch.
- Nhân viên CSKH.
- Nhân viên vận hành khu vui chơi.

### Workflow thủ công hiện tại

```text
1. Khách hàng có câu hỏi
        ↓
2. Khách tìm kiếm thông tin
        ↓
3. Không tìm thấy → liên hệ CSKH
        ↓
4. Nhân viên tìm thông tin
        ↓
5. Nhân viên trả lời khách
```

### Bước nào tốn thời gian/lỗi nhất?

**Bước 3 → 5**.

Một số câu hỏi đơn giản nhưng lặp lại nhiều lần trong ngày.

Ước tính:

```text
3–5 phút/câu hỏi
        ↓
Mục tiêu: dưới 30 giây
```

### AI có thể nhảy vào hỗ trợ ở bước nào?

LLM + RAG có thể tìm thông tin từ knowledge base:

```text
Customer Question
       ↓
LLM
       ↓
RAG Search
       ↓
Official Knowledge Base
       ↓
Answer
```

### Đo thành công bằng gì?

- Response time: **3–5 phút → dưới 30 giây**
- Tỷ lệ câu hỏi được trả lời tự động: **> 60%**
- Answer accuracy: **> 90%**
- Giảm workload CSKH: **20–30%**

### Quick Architecture

- [ ] No AI
- [ ] Rule
- [x] LLM
- [ ] Agent

### Ranh giới

AI chỉ trả lời dựa trên dữ liệu chính thức.

Nếu câu hỏi liên quan đến thanh toán, hoàn tiền, khiếu nại hoặc vấn đề ngoài knowledge base thì phải chuyển sang nhân viên.

---

# So sánh Top 3 bài toán

| Tiêu chí | Xanh SM Charging Search | Vinhomes Complaint Classification | Vinpearl AI Assistant |
|---|---:|---:|---:|
| Tần suất | Cao | Cao | Cao |
| Tác vụ lặp lại | Cao | Cao | Cao |
| Có dữ liệu đầu vào rõ ràng | Cao | Cao | Cao |
| Có thể đo metric | Rất cao | Cao | Cao |
| AI Fit | Trung bình - Cao | Cao | Cao |
| Rủi ro | Trung bình | Trung bình | Trung bình |
| Rule-based có thể giải quyết? | Có, phần chính | Có, một phần | Khó |
| LLM có giá trị? | Query understanding | Classification | Natural-language QA |
| Agent có cần thiết? | Không | Không | Chưa cần |
| Khả năng prototype | Rất cao | Cao | Cao |

---

# Quyết định AI Fit

## Bài toán 1 — Xanh SM Charging Search

### Decision: GO

Đây là bài toán phù hợp để xây dựng prototype vì input, output và metric tương đối rõ ràng.

Điểm quan trọng là không sử dụng LLM cho toàn bộ workflow.

Kiến trúc tốt hơn là:

```text
Natural Language Query
          ↓
       LLM
          ↓
Structured Parameters
          ↓
      Validation
          ↓
Geospatial Search
          ↓
Charging Station Database
          ↓
Nearest Station
```

Trong đó:

- **LLM**: hiểu query.
- **Rule/Validation**: kiểm tra radius, GPS và các giới hạn an toàn.
- **Geospatial engine**: tính khoảng cách.
- **Database**: cung cấp dữ liệu trạm sạc.
- **Application layer**: trả kết quả cho khách hàng.

### Vì sao không dùng Agent?

Agent là overkill cho phiên bản đầu tiên.

Workflow có tính deterministic cao:

```text
Input → Parse → Validate → Search → Rank → Return
```

Không cần để Agent tự suy nghĩ và tự quyết định nhiều bước.

Agent chỉ nên được cân nhắc ở phiên bản sau nếu hệ thống cần:

```text
Tìm trạm
→ Kiểm tra tình trạng trạm
→ Kiểm tra loại cổng sạc
→ Kiểm tra khả năng tương thích xe
→ Đặt lịch sạc
→ Điều hướng
→ Theo dõi quá trình sạc
```

---

# Future Flow — Xanh SM Intelligent Charging Search

```text
                    CUSTOMER
                       │
                       ▼
             "Tìm trạm sạc trong
                bán kính 5 km"
                       │
                       ▼
              ┌─────────────────┐
              │   LLM Parser    │
              │                 │
              │ Extract:        │
              │ - GPS           │
              │ - Radius        │
              │ - Intent        │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │   Validation    │
              │                 │
              │ GPS valid?      │
              │ Radius valid?   │
              └────────┬────────┘
                       │
              ┌────────┴────────┐
              │                 │
             YES               NO
              │                 │
              ▼                 ▼
      Geospatial Search    Ask Clarification
              │
              ▼
      Charging Station DB
              │
              ▼
      Calculate Distance
              │
              ▼
      Filter by Radius
              │
              ▼
      Sort by Distance
              │
              ▼
       Nearest Station
              │
              ▼
          RESPONSE
```

---

# Safety / Operational Boundary

## Rule 1 — Không bịa dữ liệu

Nếu database không có trạm sạc:

```text
Không được tự tạo tên, địa chỉ hoặc tọa độ của trạm sạc.
```

## Rule 2 — Không tự suy đoán GPS

Nếu người dùng không cung cấp vị trí và hệ thống không có location context:

```text
Ask for location.
```

## Rule 3 — Radius phải được kiểm tra

Ví dụ:

```text
User:
"Tìm trạm sạc trong bán kính 5 km."

Parsed:
radius = 5 km
```

Nếu người dùng nhập giá trị bất thường:

```text
radius = 5000 km
```

thì hệ thống phải validate và áp dụng giới hạn nghiệp vụ thay vì gửi trực tiếp xuống search engine.

## Rule 4 — LLM không quyết định khoảng cách

Khoảng cách phải được tính bằng hệ thống geospatial:

```text
GPS Customer
      +
GPS Charging Station
      ↓
Distance Calculation
```

Không dùng câu trả lời của LLM để xác định:

```text
"Trạm A cách khách hàng 2.3 km"
```

nếu con số đó chưa được backend tính toán.

## Rule 5 — Không tìm thấy trạm

Nếu:

```text
Nearest Station > Requested Radius
```

thì không được trả về trạm đó như một kết quả hợp lệ.

Hệ thống phải thông báo rằng không có trạm trong bán kính yêu cầu và có thể hỏi người dùng có muốn mở rộng bán kính hay không.

---

# Final Recommendation

Trong 3 bài toán, tôi ưu tiên **Xanh SM Intelligent Charging Search** để làm prototype.

Lý do:

1. Input rõ ràng: GPS + radius + search intent.
2. Output rõ ràng: charging station gần nhất.
3. Có thể đo lường chính xác bằng khoảng cách và thời gian phản hồi.
4. Có thể tách rõ phần AI và phần deterministic.
5. Dễ xây dựng prototype programmatically.
6. Dễ thiết lập guardrail.
7. Có thể mở rộng trong tương lai thành Agentic workflow.
8. Có thể chứng minh rằng AI Engineer biết khi nào **nên dùng LLM và khi nào không nên dùng LLM**.

### Architecture cuối cùng

```text
                    ┌─────────────────────┐
                    │      Customer       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Natural Language  │
                    │        Query        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     LLM Parser      │
                    │                     │
                    │ GPS / Radius /      │
                    │ Search Intent       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      Guardrail      │
                    │     Validation      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Geospatial Search   │
                    │                     │
                    │ Distance + Radius   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Charging Station DB │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Nearest Station     │
                    │ Selection           │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Response to User    │
                    └─────────────────────┘
```
