
# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = 30 phút/lượt**.

### Workflow hiện tại

Tài xế xe điện nhận cảnh báo pin thấp → mở app bản đồ/trạm sạc → xem các trạm gần nhưng không biết trụ nào còn trống → đi đến một trạm nhưng phát hiện đang quá tải hoặc cổng đang bận → phải rời đi, tìm trạm khác → mất thêm thời gian và năng lượng.

```text
Tài xế nhận cảnh báo pin thấp
        │
        ▼
[1] Mở app tìm trạm sạc gần                 ~ 3 phút
        │
        ▼
[2] 🔴 Xem danh sách trạm nhưng không biết trụ nào còn trống   ~ 10 phút
        │
        ▼
[3] Lái xe đến nơi, phát hiện hết trụ hoặc chờ dài       ~ 12 phút
        │
        ▼
[4] Phải tìm trạm khác, điều chỉnh lộ trình              ~ 5 phút
        │
        ▼
Tổng thời gian trung bình = 30 phút/lượt
```

### Điểm bottleneck
- **Bước 2 & 3** là bottleneck lớn nhất: thiếu thông tin thời gian thực về trạng thái từng trụ sạc, dẫn đến quyết định sai lầm và lãng phí thời gian.
- Tài xế phải đánh giá bằng kinh nghiệm cá nhân, không có hệ thống cảnh báo dự đoán lộ trình tối ưu.
- Handoff giữa “trạm sạc gần” và “trạm thực sự có thể sạc ngay” rất yếu; dữ liệu không đồng nhất.

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Tài xế VinFast và tài xế Xanh SM đang vận hành xe điện trong điều kiện pin giảm, phải tìm trạm sạc phù hợp và tránh lãng phí thời gian di chuyển. |
| **2. Current Workflow** | Tài xế nhận cảnh báo pin thấp, mở bản đồ/trạm sạc, xem danh sách trạm gần, đi đến trạm không chắc có sạc trống, phát hiện trạm quá tải hoặc hết cổng, rồi phải tìm trạm khác. Phần lớn thao tác là thủ công và dựa trên kinh nghiệm cá nhân. |
| **3. Bottleneck** | **Bước xem trạng thái trạm và lựa chọn trạm phù hợp**: tài xế không có thông tin đầy đủ về “trụ sạc nào còn trống”, “dung lượng pin còn lại có đủ để di chuyển tới trạm đó không”, và “lộ trình nào tối ưu nhất nếu trạm hiện tại quá tải”. |
| **4. Business Impact** | Mỗi lượt mất 30 phút tìm trạm không phù hợp, làm tăng thời gian chờ, giảm hiệu suất vận hành, tăng hao hụt pin và ảnh hưởng đến SLA dịch vụ cho người dùng. Nếu áp dụng vào cả đội xe lớn, tổn thất tích lũy rất cao về thời gian, chi phí nhiên liệu/điện và trải nghiệm khách hàng. |
| **5. Success Metric** | AI đạt mục tiêu: giảm thời gian tìm trạm từ **30 phút xuống dưới 8 phút/lượt**, tăng tỷ lệ tài xế tìm đúng trạm sạc có thể sạc ngay lên **≥ 85%**, giảm số lần lặp lại đến trạm sai hoặc chờ quá lâu xuống **< 10%**. |
| **6. Operational Boundary** | **AI được phép làm:** gợi ý trạm phù hợp dựa trên pin còn lại, vị trí, trạng thái thực tế, thời gian chờ ước tính và lộ trình tối ưu. **TUYỆT ĐỐI không được làm:** tự động điều khiển xe, tự quyết định lộ trình an toàn mà không có sự đồng ý của tài xế, hoặc thay đổi trạng thái hệ thống sạc. **Điểm cần duyệt:** tài xế xác nhận trạm cuối cùng, đặc biệt khi có thay đổi thời gian thực hoặc nguy cơ mất an toàn. |

---

## 3.3. Future-State Flow & AI Fit (25 min)

### AI Fit
**Mức AI Fit phù hợp:** [ ] Rule / State-Machine [ ] LLM Feature [x] Agentic Loop

Lý do: bài toán đòi hỏi phải kết hợp nhiều tín hiệu thời gian thực (pin còn lại, vị trí, trạng thái trụ, thời gian chờ, khả năng lộ trình) và liên tục cập nhật khi điều kiện thay đổi. Đây là dạng quyết định vận hành thực tế, không chỉ là một câu trả lời văn bản nên không phải LLM thuần túy; cần một agent có khả năng suy luận, tái lập lộ trình và điều chỉnh theo dữ liệu thực tế.

### Future-State Flow

```text
[1] Tài xế nhận cảnh báo pin thấp
        │
        ▼
[2] Hệ thống thu thập dữ liệu thời gian thực:
    - vị trí xe
    - pin còn lại
    - trạm sạc gần
    - trạng thái trụ sạc
    - thời gian chờ ước tính
        │
        ▼
[3] 🔵 AI Step: Dự đoán nhu cầu sạc và đề xuất 3 trạm tối ưu
        │
        ▼
[4] 🟢 Human Step (HITL): Tài xế xác nhận trạm chọn cuối cùng
        │
        ▼
[5] Hệ thống cập nhật lộ trình + cảnh báo nếu trạm sạc có biến động
        │
        ▼
[6] ↩️ Fallback: Nếu AI không tin cậy / dữ liệu thiếu, quay về:
      - trạm gần nhất theo vị trí
      - hiển thị danh sách trạm ưu tiên
      - cảnh báo người dùng kiểm tra lại trạng thái thực tế
```

### Những bước AI hỗ trợ rõ nhất
- Dự đoán lượng pin còn lại cần thiết để đi tới trạm tiếp theo.
- Đánh giá tình trạng thực tế của từng trụ sạc và thời gian chờ.
- Gợi ý tối ưu 3 trạm phù hợp nhất theo lợi ích và đường đi.
- Tái lập lộ trình nếu trạm đầu tiên quá tải hoặc đóng cửa.

### AI Step / Human Step / Fallback
- **🔵 AI Step:** Phân tích dữ liệu thời gian thực và chọn trạm tối ưu.
- **🟢 Human Step:** Tài xế xác nhận lựa chọn cuối cùng trước khi rời đi.
- **↩️ Fallback:** Nếu hệ thống không xác định được mức độ tin cậy, hệ thống tự động hiển thị trạm gần và an toàn, không tự động điều khiển xe.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán này có tính khả thi cao vì nó là bài toán vận hành thực tế, có dữ liệu đầu vào rõ ràng: vị trí xe, pin còn lại, trạng thái trạm sạc, và lịch sử di chuyển. Đây không phải bài toán “AI giải thích ngôn ngữ” mà là bài toán “quyết định tối ưu thực thi”, rất phù hợp với mô hình AI agent hỗ trợ theo thời gian thực. Rủi ro khi AI sai có thể kiểm soát được nhờ cơ chế HITL: tài xế vẫn xác nhận trạm cuối cùng, và nếu confidence thấp thì hệ thống quay về fallback dựa trên trạm gần nhất và dữ liệu an toàn. Từ góc độ chi phí, bài toán này không cần xây dựng hệ thống AI quá lớn ở giai đoạn đầu; chỉ cần prototype trên dữ liệu trạm + pin + vị trí để đo được lợi ích nhanh chóng. Với các metric rõ ràng như “giảm 30 phút/lượt xuống dưới 8 phút” và “tăng tỷ lệ tìm trạm đúng lên 85%”, đây là một dự án có giá trị kinh doanh thực tế và đủ điều kiện để bắt đầu prototype trong scope hẹp.

---