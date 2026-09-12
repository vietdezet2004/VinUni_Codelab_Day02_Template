### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | Pain từ người khác | Tài xế VinFast không nhận được cảnh báo sớm khi pin xe dưới 5%, phải tự tìm trạm sạc trong khi đang chở khách — gây hốt hoảng và trải nghiệm khách hàng tệ. |
| 2 | **VinFast** | AI có thể tốt hơn | Trợ lý AI trong xe tự động phát hiện pin nguy hiểm (< 5%), định vị trạm sạc VinFast trong bán kính 5km, soạn thông báo xin lỗi khách hàng về sự bất tiện. |
| 3 | **Xanh SM** | Pain từ người khác | Điều phối viên phải liên tục theo dõi cung–cầu theo từng khu vực và xử lý thủ công khi một zone thiếu xe hoặc ETA tăng đột biến; chỉ cần utilization giảm 1% trên quy mô ~1 triệu chuyến/ngày có thể tương đương ~10.000 chuyến/ngày capacity bị bỏ lỡ theo cách sizing đơn giản. |
| 4 | **Xanh SM** | Lặp lại | Nhân sự Fraud/Ops phải manually review các chuyến có dấu hiệu bất thường như GPS anomaly, cancel/rebook, voucher abuse, payment anomaly hoặc hành vi bất thường giữa tài xế–khách hàng; AI có thể chấm điểm rủi ro và chỉ đưa nhóm top-risk cho người kiểm tra, giảm đáng kể volume review. |
| 5 | **Xanh SM** | Tốn thời gian | Finance/Ops phải đối soát thủ công doanh thu, incentive, bonus và penalty của tài xế theo nhiều rule như khu vực, loại xe, số chuyến, acceptance/completion rate và campaign; AI có thể tự đọc policy, đối chiếu transaction và chỉ flag các exception, giảm hàng trăm đến hàng nghìn case cần xử lý thủ công mỗi kỳ. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

---

### Quick Problem Card #1 — VinFast Cảnh báo Pin Nguy Hiểm & Tìm Trạm Sạc

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Trợ lý AI trên xe VinFast tự động phát hiện pin   │
│ dưới 5%, tìm trạm sạc trong bán kính 5km, và soạn thông báo │
│ xin lỗi khách hàng về sự bất tiện.                          │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Tài xế VinFast + Khách hàng đang ngồi trên xe.            │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Xe báo pin thấp ──> Tài xế hoảng loạn nhìn đồng hồ    │
│   ──> 2. Tài xế tự mở Google Maps tìm trạm sạc thủ công     │
│   ──> 3. Tài xế tự soạn câu xin lỗi, giải thích với khách   │
│   ──> 4. Khách hàng chờ hoặc hủy chuyến, đánh giá 1 sao     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2-3 (⏱ 8–12 phút/lượt, tài xế mất tập trung lái xe) │
│ AI nhảy vào ở bước nào?                                     │
│   Tự động từ Bước 1: phát hiện ngưỡng pin → tra cứu trạm    │
│   → soạn thông báo → hiển thị để tài xế/khách xác nhận.     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian xử lý từ 10 phút ──> dưới 30 giây       │
│   - Tỉ lệ trạm sạc đề xuất đúng & khả dụng đạt >= 95%      │
│   - Tỉ lệ khách hàng nhận thông báo trong < 1 phút đạt 100% │
│                                                             │
│ Quick Architecture: [x] LLM Feature (soạn thông báo NLP)    │
│                     + Rule (ngưỡng pin 5%, bán kính 5km)     │
└─────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #2 — VinFast CSKH Soạn Thủ Công Email Xin Lỗi

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Đội CSKH VinFast phải soạn thủ công từng email/   │
│ SMS xin lỗi khách hàng bị gián đoạn hành trình do pin yếu.  │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Nhân viên CSKH VinFast (8 phút/lượt, ~50 lượt/ngày).     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhân viên CSKH nhận log sự cố hết pin từ hệ thống      │
│   ──> 2. Soạn nội dung email xin lỗi thủ công theo mẫu cũ   │
│   ──> 3. Review nội dung, điền tên khách + mã chuyến đi     │
│   ──> 4. Gửi email/SMS, ghi nhận ticket CSKH đã xử lý       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2-3 (⏱ 6–8 phút/lượt, 50 lượt/ngày = ~6 giờ/ngày)   │
│ AI nhảy vào ở bước nào?                                     │
│   Bước 2: Tự động draft email cá nhân hóa theo context sự cố │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian soạn từ 8 phút ──> dưới 1 phút/lượt     │
│   - Tiết kiệm ~5.8 giờ làm việc CSKH mỗi ngày              │
│                                                             │
│ Quick Architecture: [x] LLM Feature (draft email NLP)       │
└─────────────────────────────────────────────────────────────┘

### Quick Problem Card #3 — Xanh SM Thông Báo Hành Khách Khi Tài Xế Hết Pin

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Khi tài xế Xanh SM hết pin giữa chừng, hành      │
│ khách không được thông báo tự động → bức xúc, hủy chuyến,   │
│ đánh giá 1 sao.                                             │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│   Hành khách đặt xe Xanh SM + Đội CSKH xử lý khiếu nại.    │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tài xế hết pin, dừng xe và gọi tổng đài               │
│   ──> 2. Điều phối viên liên lạc thủ công để nắm tình huống │
│   ──> 3. Điều phối viên soạn tin nhắn gửi hành khách (thủ) │
│   ──> 4. Hành khách nhận tin muộn (>10 phút), đánh giá xấu  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│   Bước 2-3 (⏱ 10–15 phút, hành khách đã kịp tức giận)       │
│ AI nhảy vào ở bước nào?                                     │
│   Bước 3: Tự động phát thông báo + lịch xe thay thế đến app │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian hành khách nhận thông báo < 60 giây           │
│   - Giảm tỉ lệ đánh giá 1 sao vì sự cố pin từ ~30% ──> <5% │
│                                                             │
│ Quick Architecture: [x] Agent (tích hợp dispatch + notify)  │
└─────────────────────────────────────────────────────────────┘
```
