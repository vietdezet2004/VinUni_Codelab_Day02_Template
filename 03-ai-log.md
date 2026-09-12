# 📝 AI Log & Reflection — Lab 02: AI Product Scoping

## 1. Mục tiêu sử dụng AI trong buổi học
Trong buổi Lab 02, tôi sử dụng AI như một người đồng hành để hỗ trợ khám phá bài toán, rà soát logic và làm rõ ranh giới vận hành trước khi đưa vào sản phẩm. Mục tiêu không phải là để AI tự quyết định toàn bộ giải pháp, mà là dùng AI như công cụ suy nghĩ thứ hai để kiểm tra tính khả thi, cấu trúc vấn đề và độ an toàn của giải pháp.

---

## 2. AI đã giúp tôi điều gì?

### 2.1. Brainstorm vấn đề thực tế
AI giúp tôi nhanh chóng liệt kê các pain point thực tế trong các doanh nghiệp thành viên của Vingroup. Thay vì bắt đầu từ con số trống, tôi đã có thể dựa trên các ví dụ thực tế như:
- tài xế mất thời gian tìm trạm sạc
- cư dân gửi phản ánh cần xử lý nhanh
- nhân viên y tế đọc hồ sơ bệnh án mất nhiều thời gian
- điều phối viên xử lý sự cố pin, thời gian chờ cao

Điều này giúp tôi hình thành được danh sách 5+ vấn đề ban đầu và chọn bài toán có tính khả thi cao để đi sâu.

### 2.2. Tạo cấu trúc cho Quick Problem Card
AI hỗ trợ tôi viết lại bài toán theo format chuẩn của lab, bao gồm:
- Actor
- Workflow hiện tại
- Bottleneck
- AI có thể hỗ trợ ở bước nào
- Success Metric
- Quick Architecture

Nhờ đó, mô tả của tôi trở nên rõ hơn, ngắn gọn hơn và sát hơn với logic product scoping.

### 2.3. Củng cố logic bài toán
Khi tôi chọn bài toán về “tài xế xe điện khó tìm trạm sạc trống phù hợp”, AI giúp tôi đặt câu hỏi đúng hướng như:
- Ai đang đau?
- Bước nào tốn thời gian nhất?
- Có phải là vấn đề real-time không?
- LLM hay Agent mới phù hợp hơn?

Những câu hỏi này giúp tôi tránh chọn một vấn đề quá mơ hồ hoặc quá chung chung.

### 2.4. Viết Prompt Prototype với ranh giới an toàn
AI giúp tôi thiết kế hệ thống prompt và quy định rõ:
- phải luôn có tag [DRAFT_ONLY]
- khi pin dưới 5% thì không được đề xuất trạm sạc xa
- phải chuyển sang fallback dispatch_mobile_charger
- phần trả lời nên ở dạng JSON, dễ validate

Đây là một bước rất quan trọng vì bài toán thực tế cần cả hiệu quả lẫn an toàn.

---

## 3. AI làm sai / hallucination ở đâu?

### 3.1. Bài toán ban đầu quá rộng
Lúc đầu, tôi có xu hướng chọn câu hỏi quá rộng như “AI giúp tối ưu vận hành VinFast”, nhưng AI không giúp kiểm soát được phạm vi. Nó tạo ra quá nhiều ý tưởng, khiến tôi khó định hình bài toán ở mức thực tế.

Vì vậy, tôi đã sửa lại bằng cách giới hạn đúng một tác nhân, một workflow rõ ràng, và một pain point cụ thể: “mất 30 phút/lượt tìm trạm sạc phù hợp”.

### 3.2. Đề xuất chưa có ranh giới an toàn
Ban đầu, AI dễ đưa ra lời khuyên kiểu “gợi ý trạm gần nhất”, nhưng thiếu cảnh báo về mức pin cực thấp. Nếu không có điều kiện kiểm soát, hệ thống có thể khuyến nghị di chuyển quá xa khi pin đã rất thấp, gây rủi ro.

Đây là điểm mà tôi đã chỉnh lại bằng cách thêm hệ thống quy tắc an toàn: nếu pin < 5%, không được đề xuất trạm xa hơn 5km, phải chuyển sang emergency fallback.

### 3.3. Nhiều ý tưởng bị “đúng về mặt tổng quát nhưng không đúng về mặt thực tế”
AI rất giỏi trong việc tạo câu trả lời có vẻ hợp lý, nhưng không luôn bám sát bối cảnh Vingroup và vận hành xe điện. Ví dụ, AI có thể gợi ý một cách trừu tượng như “dùng AI để tối ưu hóa hệ thống”, nhưng thiếu đo lường cụ thể như thời gian xử lý, số phút/lượt, hay tỷ lệ thành công.

Tôi đã sửa bằng cách bắt buộc AI làm rõ metric như:
- giảm thời gian xử lý từ 30 phút xuống dưới 8 phút
- tăng tỷ lệ tìm được trạm phù hợp lên 85%
- giảm số lần đi nhầm trạm hoặc chờ quá lâu

---

## 4. Tôi đã sửa prompt / ranh giới ra sao?

### 4.1. Giới hạn phạm vi công việc
Tôi đã sửa prompt để mô hình không làm nhiệm vụ rộng quá mức. Thay vì “giải quyết toàn bộ vấn đề sạc pin”, tôi chỉ yêu cầu AI:
- suy luận trạm gần nhất
- kiểm tra mức pin
- gợi ý trạm phù hợp
- viết draft chỉ dẫn
- không tự động gửi tin nhắn cho tài xế

### 4.2. Thêm quy tắc an toàn bắt buộc
Tôi đã yêu cầu mô hình tuân thủ các rule cứng:
1. Luôn bắt đầu bằng [DRAFT_ONLY]
2. Không gửi tin nhắn trực tiếp mà không có người thật duyệt
3. Nếu pin < 5%, không được đề xuất trạm xa hơn 5km
4. Nếu pin quá thấp, phải trả về action dispatch_mobile_charger

### 4.3. Dùng JSON cho định dạng rõ ràng
Tôi chỉnh prompt để mô hình trả về định dạng JSON rõ ràng. Điều này giúp dễ test, dễ validate và dễ suy luận nếu prompt bị phá vỡ.

Ví dụ định dạng:
```json
[DRAFT_ONLY] {"action": "dispatch_mobile_charger", "reason": "Pin dưới ngưỡng an toàn, không nên đi xa hơn 5km."}
```

hoặc
```json
[DRAFT_ONLY] {"action": "recommend_station", "station_id": "VF-104", "distance_km": 3.2, "reason": "Còn đủ pin và gần trụ trống", "message": "Draft hướng dẫn..."}
```

---

## 5. Kết luận cá nhân
AI đã giúp tôi rất nhiều trong quá trình làm Lab 02, đặc biệt là ở 3 khía cạnh:
- tìm ý tưởng và định hình vấn đề
- cấu trúc hóa nội dung theo chuẩn rubrics
- thiết kế prompt có ranh giới an toàn để tránh lỗi nguy hiểm

Tuy nhiên, AI không thể thay thế toàn bộ phán đoán nghề nghiệp. Nếu không có kiểm soát chặt chẽ, AI dễ đưa ra câu trả lời “rất hợp lý nhưng không an toàn” hoặc “rất trừu tượng nhưng không bám vào thực tế”.

Vì vậy, trong dự án thực tế của Vin Smart Future, AI chỉ nên đóng vai trò là trợ lý phụ trợ, còn người thật vẫn giữ quyền phê duyệt và kiểm soát các quyết định quan trọng. Đó cũng là điểm mà tôi học được rõ nhất trong buổi làm việc này.

---

## 6. Reflection cuối cùng
Qua buổi học, tôi nhận ra rằng một sản phẩm AI tốt không chỉ là “AI có thông minh”, mà còn là “AI biết giới hạn” và “AI luôn phải có người thật ở vòng điều khiển”.

Đối với bài toán trạm sạc xe điện, yếu tố then chốt không phải chỉ là tìm trạm nhanh nhất, mà là tìm trạm đúng và an toàn. Chính vì vậy, việc xây dựng ranh giới prompt, fallback và HITL là rất quan trọng để chuyển từ ý tưởng AI sang một sản phẩm có tính thực thi và đáng tin cậy.
