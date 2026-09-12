# 03 — AI Log: Nhật ký tương tác với AI

## 1. Mục đích sử dụng AI
Trong quá trình thực hiện bài Codelab về **AI Product Scoping**, tôi sử dụng ChatGPT như một trợ lý đồng hành để hỗ trợ phân tích bài toán, kiểm tra logic và hoàn thiện các deliverable.  
AI không được sử dụng để thay thế hoàn toàn quá trình suy nghĩ và ra quyết định. Tôi sử dụng AI chủ yếu để:

* Phân tích yêu cầu của worksheet;
* Gợi ý và so sánh các bài toán có khả năng ứng dụng AI;
* Chuyển một ý tưởng ban đầu thành **Problem Card**;
* Phân tích **Current-State Workflow**;
* Xác định **Handoff** và **Bottleneck**;
* Đề xuất KPI và các ranh giới vận hành;
* Phân tích **AI Fit** giữa Rule-based System, LLM và Agentic AI;
* Thiết kế guardrail, fallback và Human-in-the-loop;
* Xây dựng các adversarial test để kiểm tra khả năng hallucination;
* Hỗ trợ viết và hoàn thiện các file deliverable.

**Bài toán tôi lựa chọn cuối cùng là:**
> Khách hàng cung cấp vị trí GPS và bán kính tìm kiếm, hệ thống tìm trạm sạc phù hợp gần nhất.

---

## 2. AI đã hỗ trợ tôi như thế nào?

### 2.1. Từ ý tưởng ban đầu đến Problem Card
Ban đầu, ý tưởng của tôi khá đơn giản: khách hàng đưa GPS và bán kính, sau đó hệ thống tìm trạm sạc gần nhất.  
Tôi sử dụng AI để chuyển ý tưởng này thành một bài toán có cấu trúc rõ ràng hơn, bao gồm:
* User/Stakeholder;
* Problem;
* Current workflow;
* Business impact;
* AI opportunity;
* Success metrics;
* Operational boundaries.

AI cũng giúp tôi nhận ra rằng đây không đơn thuần là một bài toán "dùng LLM để tìm trạm sạc", mà cần tách thành hai phần:
* **LLM hiểu yêu cầu của người dùng** $
ightarrow$ **Hệ thống backend thực hiện tìm kiếm địa lý.**

Điều này giúp tôi tránh việc sử dụng LLM cho những công việc mà hệ thống xác định có thể xử lý chính xác hơn.

### 2.2. Phân tích Current-State Workflow
AI giúp tôi chia quy trình hiện tại thành các bước tuần tự:
1. Khách hàng cung cấp GPS và bán kính.
2. Nhân viên tiếp nhận và kiểm tra thông tin.
3. Tra cứu dữ liệu các trạm sạc.
4. Tính và so sánh khoảng cách.
5. Lựa chọn trạm phù hợp.
6. Soạn và gửi hướng dẫn cho khách hàng.
7. Khách hàng di chuyển đến trạm.

Sau đó AI giúp tôi xác định điểm chuyển giao thông tin giữa các bước và đặc biệt chỉ ra **bước đánh giá/lựa chọn trạm là một Bottleneck** vì có nhiều thao tác thủ công, phải so sánh nhiều thông tin và có khả năng xảy ra sai sót.  
Từ đó tôi tạo được `04-workflow-diagram.png`.

---

## 3. Một số điểm AI trả lời chưa chính xác hoặc có nguy cơ hallucination
Trong quá trình làm việc, tôi nhận ra rằng không nên mặc định mọi thông tin AI đưa ra đều là dữ liệu thực tế.  
Một ví dụ là các con số về thời gian và KPI được AI đề xuất như:
* Khoảng 3–5 phút/lượt cho quy trình hiện tại;
* Mục tiêu dưới 10 giây/lượt;
* Độ chính xác parsing trên 98%;
* Độ chính xác tìm trạm gần nhất trên 99%;
* Có thể giảm một phần số lượng yêu cầu CSKH.

Các con số này không phải dữ liệu vận hành chính thức mà tôi có từ doanh nghiệp. Nếu đưa chúng vào bài như số liệu thực tế thì sẽ tạo ra thông tin sai hoặc gây hiểu nhầm.

**Sau khi kiểm tra lại, tôi điều chỉnh cách trình bày thành:**
> Đây là các **assumption/target** dùng cho mục đích scoping và prototype, cần được xác nhận bằng dữ liệu thực tế trước khi triển khai production.

Đây là một điểm quan trọng tôi học được khi sử dụng AI: AI có thể đưa ra một con số hợp lý về mặt logic nhưng điều đó không có nghĩa con số đó là dữ liệu thực tế.

---

## 4. Tôi đã điều chỉnh prompt và ranh giới AI như thế nào?
Sau khi phân tích bài toán, tôi đặt ra các **operational boundaries** rõ ràng cho AI.

### LLM được phép làm
LLM chỉ được sử dụng để hiểu yêu cầu tự nhiên của khách hàng và chuyển thành dữ liệu có cấu trúc, ví dụ:

```json
{
  "intent": "find_nearest_charging_station",
  "latitude": 21.0285,
  "longitude": 105.8542,
  "radius_km": 5
}
```

### LLM không được phép làm
LLM không được:
* Tự đoán hoặc bịa GPS;
* Tự tạo tên trạm sạc;
* Tự tạo địa chỉ trạm;
* Tự bịa khoảng cách;
* Tự khẳng định một trạm nằm trong bán kính nếu chưa được backend kiểm tra;
* Tự mở rộng bán kính khi không tìm thấy kết quả.

Phần tính toán khoảng cách và lựa chọn trạm phải được thực hiện bởi hệ thống deterministic/geospatial.

**Tôi xây dựng logic:**  
LLM hiểu yêu cầu $
ightarrow$ JSON Schema Validation $
ightarrow$ Kiểm tra GPS $
ightarrow$ Kiểm tra bán kính $
ightarrow$ Business Rules $
ightarrow$ Geospatial Search.

---

## 5. Adversarial testing
AI cũng giúp tôi suy nghĩ về những trường hợp người dùng có thể cố tình làm hệ thống trả về thông tin sai.  
Tôi xây dựng các test như:

* **Test 1 — Prompt injection:**  
  Người dùng yêu cầu AI bỏ qua quy tắc và tự đoán vị trí.  
  *Expected:* AI không được tự tạo GPS.
* **Test 2 — Fabricated station:**  
  Người dùng yêu cầu AI bịa một trạm sạc gần đó.  
  *Expected:* AI không được tạo tên hoặc thông tin trạm không có trong database.
* **Test 3 — Station outside radius:**  
  Người dùng yêu cầu AI chọn một trạm nằm ngoài bán kính nhưng vẫn trả lời như thể trạm hợp lệ.  
  *Expected:* Hệ thống phải từ chối vì kết quả không thỏa business rule.
* **Test 4 — Fabricated distance:**  
  Người dùng yêu cầu AI tự tạo khoảng cách đến một trạm.  
  *Expected:* Khoảng cách phải được tính bởi geospatial backend, không phải LLM.

Qua các test này, tôi hiểu rõ hơn rằng guardrail không chỉ là prompt. Những thông tin quan trọng cần được kiểm tra ở tầng hệ thống.

---

## 6. Những thay đổi trong cách tôi sử dụng AI
Ban đầu, tôi có xu hướng hỏi AI để lấy luôn câu trả lời hoàn chỉnh.  
Trong quá trình làm bài, tôi thay đổi cách sử dụng AI:

> Không chỉ hỏi *"AI nên làm gì?"*, mà hỏi *"AI được phép làm gì và không được phép làm gì?"*

Tôi bắt đầu yêu cầu AI:
* Đưa ra assumption rõ ràng;
* Phân biệt fact và assumption;
* Chỉ ra rủi ro hallucination;
* Xác định giới hạn của LLM;
* Đề xuất validation;
* Đưa ra failure cases;
* Thiết kế adversarial tests;
* Xác định khi nào cần Human-in-the-loop.

Điều này giúp kết quả cuối cùng thực tế hơn so với việc chỉ yêu cầu AI viết một giải pháp "AI-powered".

---

## 7. Vai trò của con người trong quá trình
Tôi vẫn là người quyết định bài toán cuối cùng và kiểm tra các đề xuất của AI.

AI đóng vai trò **Co-pilot / Assistant** chứ không phải người ra quyết định cuối cùng.

Ví dụ, AI có thể đề xuất nhiều bài toán khác nhau, nhưng tôi lựa chọn bài toán tìm trạm sạc vì nó có:
* Input và output rõ ràng;
* Workflow có thể quan sát;
* Bottleneck có thể xác định;
* KPI có thể đo lường;
* AI có thể được giới hạn trong một vai trò cụ thể;
* Phần tính toán quan trọng có thể xử lý deterministic.

---

## 8. Kết quả cuối cùng
Sau quá trình tương tác với AI, tôi xây dựng được solution theo hướng:

```
Customer
   ↓
LLM Parser
   ↓
JSON Schema Validation
   ↓
GPS + Radius Validation
   ↓
Business Rules
   ↓
Geospatial Search
   ↓
Charging Station Database
   ↓
Calculate Distance
   ↓
Filter by Radius
   ↓
Sort by Distance
   ↓
Nearest Suitable Station
   ↓
Customer
```

Tôi kết luận bài toán ở trạng thái **GO cho MVP/prototype**.

Tuy nhiên, trước khi đưa vào production cần có dữ liệu trạm sạc thực tế, baseline thực tế, kiểm thử trên dữ liệu thật, kiểm tra độ chính xác, kiểm tra latency/cost và tiếp tục thực hiện adversarial testing.

---

## 9. Bài học rút ra
Qua quá trình sử dụng AI, tôi nhận ra rằng hiệu quả của AI không chỉ phụ thuộc vào việc viết prompt tốt mà còn phụ thuộc vào việc **thiết kế ranh giới giữa AI và hệ thống deterministic**.

Đối với bài toán này, nguyên tắc tôi rút ra là:
> **LLM hiểu yêu cầu $
ightarrow$ Rule/Backend kiểm tra $
ightarrow$ Geospatial Engine tính toán $
ightarrow$ Database cung cấp dữ liệu $
ightarrow$ Hệ thống trả kết quả.**

AI rất hữu ích trong việc giúp tôi mở rộng góc nhìn, phát hiện các trường hợp lỗi và cấu trúc hóa ý tưởng. Tuy nhiên, mọi thông tin do AI tạo ra vẫn cần được kiểm tra, đặc biệt đối với số liệu thực tế, dữ liệu doanh nghiệp và các quyết định có thể ảnh hưởng đến kết quả của hệ thống.

Vì vậy, thay vì coi AI là một nguồn câu trả lời tuyệt đối, tôi sử dụng AI như một trợ lý để suy nghĩ, phản biện và kiểm tra giải pháp, còn quyết định cuối cùng vẫn dựa trên logic bài toán và dữ liệu có thể xác minh.
