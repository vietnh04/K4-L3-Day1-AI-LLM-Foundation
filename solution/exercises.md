# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *Câu trả lời của bạn*
Khi temperature tăng từ 0.0 lên 1.5, mức độ ngẫu nhiên và tính sáng tạo của mô hình tăng dần rõ rệt, nhưng tính ổn định và mạch lạc lại giảm đi. Ở mức thấp (0.0 – 0.5), mô hình trả lời mang tính chuẩn mực, cấu trúc logic và ngắn gọn; trong khi ở mức cao (1.0 – 1.5), câu trả lời bắt đầu dùng từ ngữ cảm xúc hơn, pha trộn từ vựng khác lạ (thậm chí chêm tiếng Anh) và cấu trúc câu trở nên kém ổn định hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Câu trả lời của bạn*
tôi sẽ để nó ở mức thấp, khoảng 0.0 - 0.5, để mô hình trả lời có thể chính xác nhất, đúng chuẩn mực và ngắn gọn xúc tích

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Câu trả lời của bạn*
Theo bảng giá output ($0.010 so với $0.0006 trên 1K token), GPT-4o đắt hơn GPT-4o-mini khoảng 16.7 lần (tương đương 105 USD/ngày so với 6.3 USD/ngày cho 10.5 triệu token output). GPT-4o xứng đáng chi phí cho các tác vụ suy luận phức tạp, phân tích pháp lý, y tế hoặc lập trình logic cao cấp cần độ chuẩn xác tuyệt đối. Ngược lại, GPT-4o-mini là lựa chọn tối ưu cho các tác vụ phân loại ý định (intent classification), tóm tắt tin nhắn ngắn, hoặc trả lời FAQ có kịch bản sẵn.
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Câu trả lời của bạn*

Phản hồi của persona 'giáo viên tiểu học' sử dụng văn phong gần gũi, câu ngắn và hình ảnh ẩn dụ quen thuộc như cuốn sổ tay ghi nhật ký lớp học mà ai cũng giữ một bản để không ai tẩy xóa được. Ngược lại, persona 'chuyên gia tài chính' dùng văn phong học thuật, đi thẳng vào các khái niệm chuyên sâu như sổ cái phân tán (distributed ledger), cơ chế đồng thuận (consensus mechanism) và tính bất biến (immutability). Điều này chứng minh system prompt đóng vai trò như 'chỉ thị đạo diễn', thiết lập toàn diện hệ quy chiếu kiến thức, giọng điệu và đối tượng mục tiêu trước khi model xử lý câu hỏi.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Câu trả lời của bạn*
 Kết quả thực tế qua tiktoken cho ra số token lớn hơn khoảng 15% - 25% so với công thức ước lượng thô (số từ / 0.75). Tiếng Việt tốn nhiều token hơn tiếng Anh có cùng độ dài vì các thuật toán token hóa (như BPE) được tối ưu hóa chủ yếu trên ngữ liệu tiếng Anh; các từ tiếng Việt có dấu thanh Unicode và cấu trúc từ ghép thường không có sẵn trong từ điển token, buộc mô hình phải phân tách một từ thành nhiều mảnh phụ âm, nguyên âm hoặc từng byte UTF-8 riêng biệt.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Câu trả lời của bạn*
 Streaming quan trọng nhất trong các ứng dụng hội thoại tương tác trực tiếp với con người (như trợ lý ảo, chat UI) vì nó giảm Time to First Token (TTFT), cho phép người dùng đọc nội dung ngay lập tức thay vì phải chờ đợi toàn bộ câu trả lời sinh xong. Ngược lại, non-streaming phù hợp hơn trong các tác vụ ngầm (background jobs), xử lý dữ liệu hàng loạt (batch processing), hoặc khi ứng dụng cần nhận về toàn bộ dữ liệu có cấu trúc (như định dạng JSON hợp lệ) để parse và chuyển tiếp cho một dịch vụ khác xử lý.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Câu trả lời của bạn*

 Khi hàng nghìn client cùng gặp lỗi và retry với delay cố định (ví dụ 1 giây), toàn bộ các client sẽ đồng loạt gửi lại request vào cùng một thời điểm ở giây tiếp theo, tạo ra hiệu ứng 'Thundering Herd' làm máy chủ tiếp tục bị nghẽn mạng và sập sâu hơn. Exponential backoff giải quyết triệt để vấn đề này bằng cách kéo giãn thời gian chờ theo cấp số nhân qua từng lần thử (0.1s, 0.2s, 0.4s...), giúp phân tán mật độ lưu lượng và tạo khoảng nghỉ đủ lâu cho hạ tầng máy chủ kịp giải phóng tài nguyên và tự phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Câu trả lời của bạn*
 Tôi chọn persona là 'Trợ giảng lập trình AI thân thiện và kiên nhẫn'. System prompt: 'Bạn là một trợ giảng lập trình AI thân thiện. Luôn giải thích bằng tiếng Việt dễ hiểu, đưa ra câu trả lời súc tích kèm ví dụ code ngắn gọn, và khuyến khích người học tự tư duy.' Lựa chọn 'tiếng Việt dễ hiểu' giúp đồng bộ ngôn ngữ phản hồi tự nhiên, tránh pha trộn thuật ngữ thừa; yêu cầu 'súc tích kèm ví dụ code ngắn gọn' giúp tiết kiệm token đầu ra và giúp người dùng nắm bắt giải pháp ngay mà không bị quá tải thông tin.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Câu trả lời của bạn*
 Hạn chế lớn nhất hiện tại là cửa sổ ngữ cảnh ngắn: do chỉ duy trì 3 lượt chat gần nhất (6 messages), trợ lý sẽ hoàn toàn quên đi các thông tin quan trọng được cung cấp ở đầu phiên trò chuyện. Đề xuất cải thiện: Triển khai kỹ thuật Tóm tắt ngữ cảnh (Conversation Summary Buffer). Cách triển khai: Khi lịch sử vượt quá 6 tin nhắn, thay vì xóa bỏ các tin nhắn cũ, ta gọi một model phụ (như GPT-4o-mini) để tóm tắt các lượt trao đổi cũ thành một đoạn tóm tắt ngắn gọn và lưu vào một system message phụ, từ đó vừa giữ được bộ nhớ dài hạn vừa tiết kiệm chi phí token.
---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
