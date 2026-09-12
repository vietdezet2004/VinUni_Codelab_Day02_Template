
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
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Repetitive | Điều phối xe trong giờ cao điểm bằng thao tác thủ công, so khớp đơn với tài xế và điểm đón lặp đi lặp lại hàng trăm lượt/ngày. |
| 2 | VinFast | Time-consuming | Nhân viên kiểm tra hóa đơn sạc điện, dữ liệu xe, và yêu cầu bảo hành pin thủ công dẫn đến chậm xử lý và sai lệch thông tin. |
| 3 | Vinhomes | AI-upgrade | Cư dân gửi phản ánh/đánh giá 1 sao, bộ phận chăm sóc khách hàng phải đọc, phân loại và soạn phản hồi thủ công. |
| 4 | Vinmec | Stakeholder Pain | Bác sĩ và nhân viên phải tóm tắt hồ sơ bệnh án, triệu chứng, và xác định mức độ ưu tiên bệnh nhân trước khi khám. |
| 5 | Vinpearl | Pain from other | Nhân viên chăm sóc khách du lịch xử lý liên tục yêu cầu đặt phòng, đổi lịch, và câu hỏi về dịch vụ bằng tư duy thủ công. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                      │
│                                                             │
│ Bài toán (1 câu): AI hỗ trợ gợi ý tài xế và điểm đón tốt hơn cho chuyến đi trong giờ cao điểm của Xanh SM. │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên và tài xế Xanh SM       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận đơn từ khách ──> 2. Xem vị trí và lịch xe hiện có ──> 3. Đánh giá điểm đón/điểm đến ──> 4. Gọi tài xế phù hợp ──> 5. Cập nhật trạng thái bằng tay │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Ghép đơn với tài xế phù hợp (⏱ 8-12 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Gợi ý tài xế, điểm đón và độ ưu tiên dựa trên thời gian chờ, quãng đường, mức độ tải │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian dispatch trung bình từ 10 phút xuống dưới 3 phút; tăng tỷ lệ hoàn thành chuyến từ 82% lên 90% │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                      │
│                                                             │
│ Bài toán (1 câu): AI phân loại phản ánh cư dân và đề xuất phản hồi chuẩn hóa cho Vinhomes trong vòng vài phút. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH và quản lý khu dân cư    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tiếp nhận phản ánh từ cư dân ──> 2. Đọc nội dung, đánh giá mức độ nghiêm trọng ──> 3. Chuyển cho bộ phận tương ứng ──> 4. Soạn hồi âm và theo dõi tiến độ ──> 5. Lưu hồ sơ xử lý │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Phân loại nội dung và soạn phản hồi phù hợp (⏱ 8-10 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tóm tắt vấn đề, gắn nhãn loại khiếu nại, đề xuất phản hồi và cảnh báo trường hợp cần người thật xử lý │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian phản hồi từ 10 phút xuống dưới 2 phút; tăng tỷ lệ xử lý đúng loại vụ việc lên 85% │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                      │
│                                                             │
│ Bài toán (1 câu): AI tóm tắt hồ sơ bệnh án và hỗ trợ đánh giá mức độ ưu tiên khám cho bệnh nhân tại Vinmec. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận và bác sĩ/điều dưỡng │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận hồ sơ bệnh nhân và triệu chứng ──> 2. Đọc thông tin y khoa, tiền sử bệnh ──> 3. Chấm điểm mức độ ưu tiên khám ──> 4. Chuyển cho bác sĩ phù hợp ──> 5. Ghi nhận kế hoạch điều trị │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc và tóm tắt hồ sơ bệnh án trước khi phân loại ưu tiên (⏱ 15-20 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tóm tắt triệu chứng chính, rút gọn tiền sử bệnh và đề xuất mức độ ưu tiên; bác sĩ duyệt cuối cùng │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lý hồ sơ từ 20 phút xuống dưới 5 phút; đạt tỷ lệ hỗ trợ ưu tiên chính xác trên 90% với người thật phê duyệt │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*