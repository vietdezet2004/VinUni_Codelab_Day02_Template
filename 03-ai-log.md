1. Vai trò của AI trong quá trình giải quyết bài toán
Trong quá trình thiết kế giải pháp "Tìm trạm sạc xe điện gần nhất" cho Vingroup, AI (cụ thể là Gemini) đã đóng vai trò như một người đồng nghiệp (Thought Partner) giúp tăng tốc độ xử lý ở các khía cạnh sau:

Brainstorming và định hình ý tưởng: AI giúp quét nhanh các hoạt động vận hành của VinFast và Xanh SM thông qua "4 Lenses", từ đó chỉ ra những điểm nghẽn (bottleneck) thực tế như việc tài xế mất thời gian dò dẫm tìm trạm sạc.

Cấu trúc hóa thông tin: Chuyển đổi các ý tưởng thô thành form mẫu chuẩn (Quick Problem Card, 6-field Problem Statement của Vin Smart Future).

Trực quan hóa quy trình: AI hỗ trợ đắc lực trong việc chuyển đổi luồng vận hành bằng chữ (Current-State Workflow) thành mã sơ đồ Mermaid, giúp tiết kiệm thời gian vẽ thủ công trên Draw.io.

2. Những điểm AI trả lời sai hoặc gặp "Hallucination"
Dù hỗ trợ tốt khâu cấu trúc, AI vẫn bộc lộ một số điểm yếu và thiếu sót về logic thực tế khi chưa được thiết lập ranh giới rõ ràng:

Đề xuất giải pháp "Over-engineering": Ban đầu, nếu chỉ yêu cầu "dùng AI để tìm trạm sạc", AI có xu hướng đề xuất một hệ thống tự trị (Autonomous Agent) có khả năng tự động trừ tiền trong ví điện tử của khách hàng để "đặt cọc" trụ sạc. Đây là một rủi ro lớn (hallucination về quyền hạn) vì vi phạm luồng thanh toán thực tế và dễ gây lỗi bồi hoàn.

Bỏ qua rào cản vật lý: AI ban đầu gợi ý thuật toán chỉ dựa trên "khoảng cách địa lý gần nhất" (GPS ngắn nhất) mà quên mất các yếu tố vật lý thiết yếu của xe điện như: % pin còn lại có đủ để lết tới trạm đó không, công suất trụ sạc có tương thích với dòng xe không, và tình trạng kẹt xe hiện tại.

Hạn chế về đa phương tiện: AI văn bản không thể trực tiếp xuất ra file ảnh (PNG/JPG) khi được yêu cầu "vẽ sơ đồ", mà chỉ có thể thay thế bằng việc xuất mã code (Mermaid) hoặc ascii art.

3. Quá trình điều chỉnh Prompt và Ranh giới (Boundaries)
Để khắc phục các lỗi trên và đưa AI đi đúng hướng, tôi đã phải liên tục tinh chỉnh các câu lệnh (Prompt Engineering) và đặt ra các rào cản kỹ thuật nghiêm ngặt:

Bổ sung Operational Boundary (Ranh giới vận hành): Đưa ra chỉ thị rõ ràng trong Prompt: "CẤM: AI không được tự động gửi tin đi mà không có điều phối viên phê duyệt (Bắt buộc HITL)" và "Tuyệt đối không được gợi ý trạm sạc có khoảng cách xa hơn quãng đường tối đa xe có thể đi, không gợi ý trụ sạc sai công suất".

Ép khuôn định dạng (Few-shot prompting): Cung cấp sẵn một khung bảng mẫu (6-field Problem Statement) và một ví dụ hoàn chỉnh của luồng Xanh SM để AI mô phỏng theo, thay vì để AI tự do viết văn xuôi dài dòng.

Chuyển đổi công cụ linh hoạt: Khi AI không thể tạo ảnh sơ đồ, tôi đã điều chỉnh prompt thành "cho mã code để vẽ lên draw.io", tận dụng thế mạnh viết code của AI để làm cầu nối sang một phần mềm trực quan hóa khác.