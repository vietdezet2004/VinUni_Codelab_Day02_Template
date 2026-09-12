# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|---------------------------------|------|---------------------|
| 1 | VinFast                         | Lặp lại (Repetitive)| Nhân viên kế toán/vận hành phải thủ công kiểm tra và  so khớp hàng ngàn hóa đơn giao dịch sạc điện tại các trạm sạc với hệ thống thanh toán ngân hàng mỗi ngày.|
| 2 | Vinhomes| Tốn thời gian (Time-consuming)| Đội ngũ CSKH mất nhiều thời gian đọc, phân loại và soạn thảo văn bản phản hồi từng đánh giá 1 sao của cư dân liên quan đến vấn đề phí dịch vụ hoặc bảo trì thang máy.|
| 3 | Vinpearl| AI có thể tốt hơn (AI-upgrade)| Chatbot hiện tại của VinWonders chỉ trả lời rập khuôn theo kịch bản có sẵn, chưa hỗ trợ tư vấn lộ trình vui chơi cá nhân hóa hoặc xử lý khiếu nại đổi vé phức tạp cho khách du lịch.|
| 4 | Xanh SM| Pain từ người khác (Stakeholder Pain)| Tài xế xe điện khó tìm trạm sạc trống phù hợp nhất theo dung lượng pin còn lại và tình trạng thực tế của trụ sạc.|
| 5 | Vinmec| Tốn thời gian (Time-consuming)| Bác sĩ và điều dưỡng tốn nhiều thời gian nhập liệu thủ công các triệu chứng của bệnh nhân vào hệ thống bệnh án điện tử (EMR) sau mỗi ca khám.|

---
# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động đối chiếu và so khớp hàng ngàn    │
│ giao dịch sạc điện với hệ thống thanh toán ngân hàng        │
│ Công ty thành viên: [x] VinFast   [ ] Xanh SM   [ ] Vinhomes│
│                     [ ] Vinmec    [ ] Khác (Ghi rõ)________ │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên kế toán / Tài chính VinFast  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xuất file log giao dịch từ trụ sạc ──>                 │
│   2. Tải sao kê từ các ngân hàng/ví điện tử ──>             │
│   3. Dùng Excel/thủ công lọc và khớp mã giao dịch ──>       │
│   4. Xử lý các dòng bị lệch/sai số tiền thủ công            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 120 phút/ngày)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Đọc dữ liệu hóa đơn/  │
│ sao kê đa định dạng (OCR + LLM) và tự động match dòng lệch. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   VD: "Giảm thời gian đối soát từ 120 min ──> under 15 min" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Đội ngũ CSKH mất nhiều thời gian xử lý và │
│ soạn thảo phản hồi thủ công cho từng đánh giá 1 sao của cư dân│
│ Công ty thành viên: [ ] VinFast   [ ] Xanh SM   [x] Vinhomes│
│                     [ ] Vinmec    [ ] Khác (Ghi rõ)________ │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên Chăm sóc khách hàng Vinhomes│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc khiếu nại của cư dân trên app/fanpage ──>          │
│   2. Xác minh lỗi với bộ phận kỹ thuật tòa nhà ──>          │
│   3. Soạn thảo email/tin nhắn xin lỗi và giải thích ──>     │
│   4. Gửi phản hồi cho cư dân                                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ 15 phút/lượt)     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tóm tắt lỗi, phân loại│
│ sentiment và generate sẵn văn bản phản hồi chuẩn mực dựa    │
│ trên cơ sở dữ liệu (KB) của tòa nhà.                        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   VD: "Giảm thời gian soạn phản hồi từ 15 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tài xế xe điện khó tìm trạm sạc trống phù │
│ hợp nhất theo dung lượng pin còn lại và tình trạng thực tế  │
│ Công ty thành viên: [x] VinFast   [ ] Xanh SM   [ ] Vinhomes│
│                     [ ] Vinmec    [ ] Khác (Ghi rõ)________ │
│                                                             │
│ Ai đang đau (Actor)? Tài xế VinFast & Tài xế Xanh SM        │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xe sắp hết pin, tài xế mở app bản đồ tìm trạm sạc ──>   │
│   2. Xem danh sách các trạm gần nhưng không rõ trụ trống ──>│
│   3. Lái xe đến nơi mới phát hiện hết trụ sạc hoặc đang chờ dài──>│
│   4. Tiếp tục phải tìm đường sang trạm sạc khác             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 30 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dự báo lượng pin tiêu │
│ hao, quét trạng thái thời gian thực của cổng sạc và gợi ý   │
│ lộ trình tối ưu có sẵn trụ trống.                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   VD: "Giảm thời gian chờ đợi tại trạm sạc từ 30 min ──> 0 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘

---

