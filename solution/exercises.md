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
Temperature càng tăng thì câu trả lời càng biến hóa và sáng tạo nhưng giảm tính ổn định. Ở mức 0.0 câu trả lời luôn cố định và an toàn; mức 0.5–1.0 câu chữ tự nhiên và phong phú; còn ở mức 1.5 câu văn bắt đầu lạ lẫm, bay bổng và dễ bị lan man.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Câu trả lời của bạn*
tôi sẽ để nó ở mức thấp, khoảng 0.0 - 0.5, để mô hình trả lời có thể chính xác nhất, đúng chuẩn mực và ngắn gọn súc tích tránh gây ảo giác

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Câu trả lời của bạn*
GPT-4o đắt hơn GPT-4o-mini khoảng 16.7 lần. Nên dùng GPT-4o khi cần giải quyết bài toán khó, viết code phức tạp hoặc tư vấn chuyên sâu. Nên dùng bản mini cho các tác vụ đơn giản như phân loại tin nhắn, tóm tắt ngắn hoặc trả lời câu hỏi thường gặp.
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

Bản "giáo viên tiểu học" dùng từ ngữ mộc mạc, ví blockchain như cuốn sổ ghi chép chung của cả lớp để trẻ dễ hiểu. Bản "chuyên gia tài chính" đi thẳng vào các thuật ngữ như sổ cái phân tán, cơ chế đồng thuận và tính bất biến. System prompt đóng vai trò như bản phân vai, quyết định phong cách xưng hô, độ sâu kiến thức và đối tượng nghe của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Câu trả lời của bạn*
Số token đếm thật qua tiktoken cao hơn công thức ước lượng (số từ / 0.75) khoảng 15% đến 25%. Tiếng Việt tốn nhiều token hơn tiếng Anh vì có dấu thanh Unicode và nhiều từ ghép; bộ đếm vốn tối ưu cho tiếng Anh nên một từ tiếng Việt thường bị chẻ nhỏ thành nhiều mảnh token.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Câu trả lời của bạn*
Streaming quan trọng nhất khi làm ứng dụng chat trực tiếp với người dùng, vì chữ nhảy ra từng từ giúp họ đọc ngay mà không phải nhìn màn hình chờ đợi. Non-streaming phù hợp hơn khi chạy ngầm (batch job) hoặc khi cần nhận về trọn vẹn một file JSON để phần mềm tự động đọc tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Câu trả lời của bạn*
Nếu dùng thời gian chờ cố định (ví dụ 1 giây), hàng nghìn máy cùng bị lỗi sẽ đồng loạt gọi lại API ở giây tiếp theo, khiến máy chủ tiếp tục nghẽn và sập nặng hơn (hiệu ứng dồn toa). Exponential backoff kéo giãn thời gian chờ tăng dần (0.1s, 0.2s, 0.4s...) giúp giãn cách các request và cho máy chủ có thời gian hồi phục.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Câu trả lời của bạn*
Persona tôi chọn: "Trợ giảng lập trình AI thân thiện". System prompt: "Bạn là trợ giảng AI thân thiện, trả lời ngắn gọn bằng tiếng Việt dễ hiểu và luôn kèm ví dụ code ngắn." Yêu cầu "ngắn gọn" giúp tiết kiệm chi phí token; yêu cầu "kèm ví dụ code" giúp người học dễ hiểu và áp dụng được ngay.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Câu trả lời của bạn*
Hạn chế lớn nhất là bot chỉ nhớ được 3 lượt chat gần nhất (6 tin nhắn), nếu chat dài thì các thông tin giới thiệu ban đầu sẽ bị quên sạch. Cải thiện: Dùng cơ chế tóm tắt lịch sử (Summary Buffer) — khi chat quá dài, ta gọi model nhỏ tóm tắt các trao đổi cũ thành một đoạn văn ngắn và lưu lại, vừa nhớ lâu vừa tiết kiệm token.
---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
