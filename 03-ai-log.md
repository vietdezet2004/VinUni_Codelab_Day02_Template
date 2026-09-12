# Bài tự luận: Phản ánh quá trình sử dụng AI làm trợ lý đồng hành

Trong quá trình thực hiện bài tập tìm kiếm pain point vận hành và thiết kế quy trình ứng dụng AI, tôi sử dụng ChatGPT như một trợ lý đồng hành thay vì xem AI là công cụ đưa ra đáp án hoàn chỉnh. AI hỗ trợ tôi ở nhiều khâu, từ brainstorm bài toán, cấu trúc hóa pain point, phân tích quy trình hiện tại đến xây dựng sơ đồ Current-State Workflow. Tuy nhiên, quá trình này cũng cho tôi thấy rõ một vấn đề quan trọng: AI có thể tạo ra câu trả lời rất thuyết phục nhưng không đồng nghĩa với việc tất cả thông tin đều chính xác hoặc có nguồn gốc từ dữ liệu đầu vào.

## 1. AI đã giúp tôi những gì?

Đầu tiên, AI giúp tôi mở rộng góc nhìn khi tìm kiếm các pain point vận hành cho Xanh SM và VinFast. Thay vì chỉ nghĩ đến những bài toán AI phổ biến như chatbot hay trợ lý hỏi đáp, AI gợi ý các quy trình có tính thủ công, lặp lại hoặc có nhiều điểm nghẽn như xử lý khiếu nại, điều phối tài xế, phát hiện bất thường và đối soát incentive.

Sau đó, AI giúp tôi chuyển các ý tưởng tương đối chung thành những bài toán có cấu trúc rõ ràng hơn. Ví dụ, với tình huống xe VinFast báo pin dưới 5%, AI giúp phân tách thành actor, workflow hiện tại, bottleneck, business impact, success metric và operational boundary. Nhờ vậy, một ý tưởng ban đầu đã trở thành một problem statement có thể tiếp tục phát triển thành prototype.

AI cũng đặc biệt hữu ích trong việc trực quan hóa quy trình. Từ nội dung trong Problem Scan và Deep-Dive Report, tôi yêu cầu AI thể hiện Current-State Workflow với các bước tuần tự, handoff, thời gian xử lý và bottleneck. Kết quả giúp tôi nhìn rõ hơn rằng vấn đề không chỉ nằm ở việc "xe hết pin", mà nằm ở chuỗi thao tác sau đó: tài xế tự tìm trạm sạc, tự lựa chọn phương án, tự giải thích với khách hàng và cuối cùng khách hàng phải chờ hoặc hủy chuyến.

Một giá trị khác của AI là khả năng chuyển đổi cùng một nội dung sang nhiều định dạng. Ví dụ, từ nội dung phân tích dạng văn bản, AI có thể tạo bảng Markdown, flowchart và sơ đồ trực quan. Điều này giúp tôi tiết kiệm thời gian trình bày và tập trung nhiều hơn vào việc kiểm tra tính hợp lý của bài toán.

## 2. AI đã trả lời sai hoặc hallucination ở đâu?

Điểm đáng chú ý nhất là khi tôi yêu cầu AI tìm các pain point và đưa ra con số thống kê ước tính. Ở một số câu trả lời ban đầu, AI đã bổ sung những thông tin như quy mô khoảng 1 triệu chuyến/ngày, doanh thu hàng năm và các mức tổn thất tính theo tỷ đồng. Những con số này nghe có vẻ hợp lý và được trình bày rất tự tin, nhưng không phải tất cả đều xuất phát từ hai tài liệu mà tôi đang sử dụng cho bài tập.

Đây là một dạng hallucination hoặc ít nhất là một vấn đề về provenance: AI trộn lẫn thông tin từ nguồn bên ngoài, suy luận của mô hình và dữ liệu trong tài liệu thành một câu trả lời duy nhất. Nếu tôi đưa nguyên câu trả lời đó vào bài mà không kiểm tra, người đọc rất khó phân biệt đâu là số liệu thực tế, đâu là estimate và đâu là giả định của AI.

Một ví dụ khác là khi AI phân tích workflow, nếu không quy định rõ phạm vi, AI có xu hướng "cải thiện" quy trình ngay trong phần Current-State. Điều này có thể dẫn đến việc đưa các thành phần AI, automation hoặc API vào quy trình hiện tại, trong khi mục tiêu của Current-State là mô tả đúng cách con người đang làm việc trước khi có giải pháp AI.

Qua đó, tôi nhận ra rằng câu trả lời càng chi tiết và chuyên nghiệp thì càng cần kiểm chứng. Hình thức trình bày đẹp không phải là bằng chứng cho tính chính xác của nội dung.

## 3. Tôi đã sửa prompt và thiết lập ranh giới như thế nào?

Sau khi nhận thấy vấn đề trên, tôi thay đổi cách làm việc với AI theo hướng "AI hỗ trợ phân tích, con người kiểm soát sự thật".

Thay vì yêu cầu chung chung như:

> "Hãy tìm pain point và đưa ra số liệu tổn thất."

tôi chuyển sang yêu cầu có phạm vi rõ ràng hơn, ví dụ:

> "Chỉ sử dụng thông tin có trong hai file được cung cấp. Không tự bổ sung số liệu bên ngoài. Nếu một con số là estimate thì phải ghi rõ là estimate. Không đưa giải pháp AI vào Current-State Workflow."

Tôi cũng chia bài toán thành từng bước nhỏ thay vì yêu cầu AI làm tất cả cùng một lúc. Quy trình làm việc của tôi trở thành:

**Tài liệu nguồn → AI trích xuất → Tôi kiểm tra → AI cấu trúc hóa → Tôi xác nhận → AI trực quan hóa.**

Đối với Current-State Workflow, tôi đặt ranh giới đặc biệt rõ:

* Chỉ mô tả quy trình thủ công hiện tại.
* Không đưa AI vào Current-State.
* Mỗi bước phải có actor cụ thể.
* Handoff phải được đánh dấu rõ.
* Bottleneck phải dựa trên nội dung đã có trong tài liệu.
* Thời gian xử lý phải lấy từ tài liệu; nếu không có thì không tự bịa.
* Phân biệt rõ **Current-State** và **Future-State**.
* Các con số ước tính phải được gắn nhãn là ước tính.

Đây cũng là cách tôi kiểm soát rủi ro khi AI đề xuất giải pháp. Trong Deep-Dive Report, phạm vi được giới hạn khá rõ: hệ thống được phép truy xuất GPS xe và thông tin trạm sạc, tạo bản nháp thông báo và yêu cầu tài xế xác nhận trước khi gửi; đồng thời có fallback nếu không có trạm phù hợp. Việc đặt operational boundary như vậy giúp tránh để AI tự động thực hiện một hành động có rủi ro cao.

## 4. Bài học lớn nhất tôi rút ra

Điều quan trọng nhất tôi học được là **không nên xem AI là nguồn sự thật tuyệt đối**. AI rất giỏi trong việc tổng hợp, cấu trúc hóa, diễn đạt và phát hiện những hướng tiếp cận mà con người có thể bỏ sót. Nhưng AI không tự biết đâu là dữ liệu nội bộ chính xác nếu tôi không cung cấp hoặc kiểm soát nguồn.

Vì vậy, vai trò của tôi thay đổi từ "người hỏi AI để lấy đáp án" thành "người điều phối và kiểm định AI".

Tôi có thể giao cho AI những việc như brainstorm, phân loại, viết lại, lập bảng, tạo workflow hoặc đề xuất cách đo KPI. Nhưng đối với những nội dung quan trọng như số liệu tổn thất, quy trình thực tế, business impact hay operational boundary, tôi phải quay lại tài liệu nguồn để xác nhận.

Đặc biệt, tôi nhận ra rằng một prompt tốt không chỉ nói AI **phải làm gì**, mà còn phải nói rõ AI **không được làm gì**. Những ranh giới như "chỉ dùng dữ liệu trong file", "không tự bổ sung số liệu", "đánh dấu estimate" hay "không đưa AI vào Current-State" đã giúp kết quả cuối cùng đáng tin cậy hơn đáng kể.

## 5. Kết luận

Sau quá trình làm việc, tôi đánh giá AI mang lại hiệu quả lớn nhất khi được sử dụng như một **trợ lý đồng hành có kiểm soát**. AI giúp tôi đi nhanh hơn từ một ý tưởng mơ hồ đến một workflow có cấu trúc, đồng thời hỗ trợ chuyển đổi nội dung thành những dạng trình bày dễ hiểu hơn.

Tuy nhiên, chất lượng cuối cùng vẫn phụ thuộc vào khả năng đặt câu hỏi, kiểm tra nguồn và thiết lập ranh giới của người sử dụng. Hallucination không chỉ là việc AI "bịa" một thông tin rõ ràng sai; nó còn có thể xuất hiện khi AI trộn lẫn dữ liệu nguồn với suy luận và thông tin bên ngoài mà không nói rõ nguồn gốc.

Do đó, nguyên tắc tôi rút ra sau bài tập này là:

**AI tạo tốc độ và mở rộng khả năng tư duy; con người chịu trách nhiệm về sự thật, phạm vi và quyết định cuối cùng.**

Đây cũng là cách tôi muốn tiếp tục sử dụng AI trong công việc AI Engineer: không chỉ hỏi "AI có thể làm được gì?", mà quan trọng hơn là xác định **AI được phép làm gì, dựa trên dữ liệu nào, ở bước nào của quy trình và con người phải kiểm soát phần nào**.
