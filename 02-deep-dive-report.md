# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping 
Quy trình xử lý sự cố hết pin thực địa hiện tại của điều phối viên Xanh SM:

┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Bước 1       │    │ Bước 2       │    │ Bước 3       │    │ Bước 4       │
│ Nhận cảnh báo│    │ Tra cứu trạm │    │ Lái xe theo  │    │ Xác nhận thực│
│ pin yếu trên │ ──→│ trên App và  │ ──→│ chỉ dẫn GPS  │ ──→│ tế (Trống /  │
│ xe (< 20%)   │    │ chọn bừa     │    │ đến trạm     │    │ Hỏng / Kín xe│
│ Ai: Tài xế   │    │ Ai: Tài xế   │    │ Ai: Tài xế   │    │ Ai: Tài xế   │
│ ⏱ 2 phút     │    │ ⏱ 3 phút 🔴  │    │ ⏱ 15 phút    │    │ ⏱ 5 phút 🔴  │
│ In: Màn hình │    │ In: App/Map  │    │ In: Toạ độ   │    │ In: Tại trạm │
│ Out: Mở App  │    │ Out: Điểm đến│    │ Out: Tới nơi │    │ Out: Status  │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                                              │
                                                              ▼
                                                       ┌──────────────┐
                                                       │ Bước 5       │
                                                       │ Chờ xếp hàng │
                                                       │ hoặc quay lại│
                                                       │ Bước 2 tìm   │
                                                       │ Ai: Tài xế   │
                                                       │ ⏱ 15 phút 🔴 │
                                                       │ In: Quyết định│
                                                       │ Out: Cắm sạc │
                                                       └──────────────┘

🔴 = Bottlenecks (Thiếu dữ liệu trống/hỏng real-time của trụ sạc, rủi ro đến nơi không sạc được).
⏱ Tổng thời gian tìm kiếm & chờ đợi: ~40 phút/lượt (chưa tính thời gian cắm sạc thực tế).

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Tài xế điều khiển xe điện (Khách hàng VinFast & Tài xế Xanh SM). |
| **2. Current Workflow** | Khi xe báo pin yếu (< 20%), tài xế tự mở màn hình/ứng dụng bản đồ, tra cứu danh sách các trạm sạc gần đó, chọn bừa một trạm theo cảm tính và lái xe tới. Nếu đến nơi trụ sạc bị hỏng hoặc kín xe, tài xế phải chờ đợi hoặc lặp lại quy trình tìm kiếm. 5 bước, thủ công, mất ~30-40 phút/lượt tìm và chờ. |
| **3. Bottleneck** | Bước 2 & 4 (mất 20-30 phút): Việc chọn trạm sạc hoàn toàn thiếu dữ liệu thời gian thực (real-time) về số lượng trụ đang trống, tình trạng lỗi phần cứng của trụ, và số lượng xe đang xếp hàng chờ tại trạm đó. |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup.Gây ra tâm lý "range anxiety" (sợ cạn pin giữa đường) và trải nghiệm tồi tệ cho chủ xe cá nhân VinFast. Đối với tài xế Xanh SM, việc vòng vo tìm trạm hoặc chờ đợi sạc gây lãng phí thời gian vàng ngọc, làm giảm số cuốc xe chạy được trong ngày và rò rỉ doanh thu đáng kể. |
| **5. Success Metric** | 1. Giảm tổng thời gian tìm kiếm và chờ đợi tại trạm sạc từ 40 phút xuống dưới 5 phút (Efficiency). 2. Tỉ lệ điều hướng thành công đến trạm có trụ sạc đang trống và sẵn sàng cắm sạc đạt trên 95% (Quality). |
| **6. Operational Boundary** | AI được phép truy xuất dữ liệu phần cứng xe (% pin còn lại, tốc độ tiêu hao), toạ độ GPS, tình trạng giao thông và API trạng thái trạm sạc VinFast. CẤM: Tuyệt đối không được gợi ý trạm sạc có khoảng cách xa hơn quãng đường tối đa xe có thể đi với lượng pin hiện tại; không gợi ý trụ sạc có công suất không tương thích với dòng xe. |

## 3.3. Future-State Flow & AI Fit 
* **AI Fit:** Chọn AI Agent (Intelligent Routing Agent). Vì bài toán này cần một Agent chạy ngầm, liên tục thu thập dữ liệu động (% pin thực tế, vị trí xe, tình trạng kẹt xe, status từng trụ sạc) để đưa ra phán đoán logic và thậm chí tự động "giữ chỗ" (booking) trụ sạc ngay khi tài xế đồng ý.

* **Quy trình tương lai (Future-State):**

┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Bước 1       │    │ Bước 2       │    │ Bước 3       │    │ Bước 4       │
│ Hệ thống nhận│    │ 🔵 AI Agent  │    │ 🔵 AI popup  │    │ 🟢 Tài xế    │
│ diện pin yếu │ ──→│ phân tích pin│ ──→│ 1 gợi ý trạm │ ──→│ bấm xác nhận,│
│ (< 20%)      │    │ & API trụ sạc│    │ trống tối ưu │    │ AI dẫn đường │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                                              │
                                                              ▼
                                                       ↩️ Fallback:
                                                       Nếu AI mất kết nối 
                                                       với máy chủ trạm sạc,
                                                       tài xế dùng map tìm 
                                                       thủ công như cũ.
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
> Về mặt kỹ thuật (Technical Feasibility): Vingroup sở hữu trọn vẹn hạ tầng (Phần cứng xe - Trạm sạc - Ứng dụng phần mềm). Việc xây dựng một AI Agent liên tục gọi các API (Google Maps Traffic + Trạng thái trụ sạc VinFast + Mức tiêu hao pin thực tế của xe) để tính toán ma trận điểm đến là hoàn toàn khả thi với công nghệ hiện tại, không gặp rào cản về việc xin phép bên thứ ba.

> Về mặt chi phí và lợi ích (ROI & Cost-benefit): Chi phí phát triển Prototype AI Agent (Cloud, LLM API, Routing API) là cực kỳ nhỏ so với chi phí cơ hội (Opportunity Cost) đang bị lãng phí. Đối với Xanh SM, mỗi tài xế tiết kiệm được 30 phút chờ sạc/ngày đồng nghĩa với việc tăng thêm tối thiểu 1-2 cuốc xe/ngày, nhân với quy mô hàng chục ngàn xe sẽ mang lại mức tăng trưởng doanh thu khổng lồ. Đối với mảng bán lẻ VinFast, đây là một "Killer Feature" giúp đánh bay điểm yếu lớn nhất của xe điện (việc sạc pin), nâng tầm trải nghiệm khách hàng và tạo lợi thế cạnh tranh tuyệt đối. Tóm lại, đây là dự án có rủi ro thấp nhưng mang lại High Impact - High ROI.

---
