# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Tài xế VinFast (người lái xe điện có hành khách) và Hành khách đang ngồi trên xe. |
| **2. Current Workflow** | Khi xe báo pin < 5%, tài xế tự mở Google Maps tìm trạm sạc gần nhất (~5–8 phút), tự soạn câu xin lỗi miệng với khách (~2 phút), khách chờ hoặc tự hủy chuyến. Hoàn toàn thủ công, không có thông báo chính thức. |
| **3. Bottleneck** | Bước tìm kiếm trạm sạc khả dụng trong bán kính 5km phù hợp loại cổng sạc xe (⏱ 5–8 phút) và soạn thông báo xin lỗi khách lịch sự + đề xuất phương án (⏱ 2–3 phút). Tổng ~10 phút, tài xế mất tập trung, khách không được thông báo kịp thời. |
| **4. Business Impact** | ~80 sự cố/ngày tại TP.HCM & Hà Nội. Gây lãng phí ~13 giờ/ngày (tài xế không chở khách trong lúc tìm trạm). Tỉ lệ review 1 sao liên quan pin yếu chiếm ~30% tổng review tiêu cực VinFast. Tổn thất ước tính ~15 triệu VND doanh thu/ngày do xe dừng tìm trạm. |
| **5. Success Metric** | 1. Thời gian từ khi pin chạm 5% đến khi tài xế nhận thông tin trạm sạc gần nhất: < 30 giây. 2. Tỉ lệ trạm đề xuất đúng loại cổng sạc & còn trụ trống: ≥ 95%. 3. Hành khách nhận thông báo xin lỗi + ETA trạm sạc trong < 60 giây: 100%. |
| **6. Operational Boundary** | **ĐƯỢC PHÉP:** Truy xuất API GPS xe, API trạm sạc VinFast (trạng thái trụ, loại cổng, bán kính 5km), soạn thảo thông báo xin lỗi dạng `[DRAFT_ONLY]` hiển thị trên màn hình xe để tài xế xác nhận. **TUYỆT ĐỐI CẤM:** Gửi thông báo đến khách hàng mà không có tài xế/điều phối viên xác nhận; đề xuất trạm sạc ngoài 5km khi pin < 5% (nguy cơ cạn pin giữa đường); tiết lộ tọa độ GPS chính xác của hành khách ra ngoài hệ thống VinFast. |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** `[x] LLM Feature` — Quy trình có cấu trúc cố định (pin ngưỡng → tìm trạm → soạn thông báo), không cần Agent tự trị vì rủi ro khi đề xuất sai trạm sạc rất cao (tài xế hết pin giữa đường).
* **Vẽ Future-State Flow:**

```
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ Bước 1           │    │ Bước 2           │    │ Bước 3           │    │ Bước 4           │
│ Cảm biến xe phát │    │ 🔵 AI Auto-pull  │    │ 🔵 LLM soạn      │    │ 🟢 Tài xế bấm   │
│ hiện pin < 5%    │──→ │ GPS + danh sách  │──→ │ [DRAFT_ONLY]     │──→ │ "XÁC NHẬN" trên  │
│                  │    │ trạm sạc ≤ 5km   │    │ thông báo xin    │    │ màn hình xe để   │
│ Trigger tự động  │    │ còn trụ trống    │    │ lỗi + chỉ đường  │    │ gửi cho khách    │
└──────────────────┘    └──────────────────┘    └──────────────────┘    └──────────────────┘
                                                                               │
                                                                               ▼
                                                                    ↩️ Fallback:
                                                                    Nếu không có trạm ≤ 5km
                                                                    → JSON dispatch_mobile_charger
                                                                    + Tài xế tự xử lý thủ công
```

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? *(VinFast có API trạm sạc thực, GPS xe thực)*
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? *(Tài xế phải xác nhận trước khi gửi — HITL bắt buộc; Fallback = xe cứu hộ di động khi không có trạm ≤ 5km)*
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? *(Tài xế được giảm tải, khách được thông báo nhanh — win-win rõ ràng)*

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán có **scope hẹp, rõ ràng**: một trigger duy nhất (pin < 5%), hai output cố định (trạm sạc ≤ 5km + thông báo xin lỗi). Metric thành công đo được ngay (thời gian < 30s, tỉ lệ đề xuất đúng ≥ 95%). Rủi ro được kiểm soát hoàn toàn qua HITL (tài xế xác nhận trước khi gửi) và Fallback cứng (dispatch xe sạc di động). Giải pháp LLM Feature đơn giản hơn Agent — phù hợp với môi trường xe điện realtime. Tổng chi phí vận hành thấp, tác động đến trải nghiệm khách hàng cao và đo được ngay qua tỉ lệ review 1 sao. 