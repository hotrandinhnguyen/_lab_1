# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Về thời gian (latency): Temperature cao khiến AI phản hồi nhanh hơn do nói ngẫu nhiên và dễ sinh ra hallucination (như mức temperature = 1.5 chỉ mất 2.6s để nói về cà phê), còn ở mức temperature thấp AI tốn thời gian hơn (4.12s với temperature = 0.0, 3.7s với temperature = 1.0 và 3.9s với temperature = 0.5 để nói vể hang Sơn Đoòng) để sinh ra một văn bản chuẩn thông tin hơn.
> Về văn bản được sinh ra: Khi temperature thấp (0.0), câu trả lời chuẩn xác nhưng rập khuôn. Khi temperature cao (1.0 - 1.5), văn phong sáng tạo hơn nhưng dễ lạc đề và bị ảo giác (hallucination) lặp từ.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature ở mức từ 0.0 đến 0.2. 
> Lý do: vì chatbot CSKH cần ưu tiên sự chính xác trong việc cung cấp các thông tin như chính sách hay thông tin sản phẩm,... và tránh sinh ra những thông tin sai lệch để tránh rủi ro pháp lý và uy tín cho công ty.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> GPT-4o đắt hơn GPT-4o-mini khoảng gần 16.67 lần ($0.010 so với $0.0006 trên 1K token đầu ra). Cụ thể với ước tính 10.5M output tokens/ngày (10k * 3 * 350), GPT-4o tốn gần $105/ngày còn GPT-4o-mini chỉ tốn gần $6.3/ngày.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> Sử dụng GPT-4o là xứng đáng khi hệ mô hình cần phải giải quyết những bài toán, công việc phức tạp và cần độ chính xác cao: VD như trợ lý luật pháp tư vấn hợp đồng, phân tích báo cáo y tế (cần độ chính xác cao).
> Chọn GPT-4o-mini là lựa chọn tốt hơn đối với các nhiệm vụ lặp đi lặp lại không đòi hỏi độ chính xác quá cao, cần ưu tiên tốc độ và chi phí thấp: VD như dịch thuật, tóm tắt nội dung, trích xuất dữ liệu.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng bậc nhất khi tương tác giao diện người dùng trực tiếp. Bởi vì lúc này thay vì cần chờ đợi LLM sinh ra hết tất cả token (ví dụ 20s) sau đó trả về người dùng mới thấy được (tức là phải đợi 20s mới có chữ xuất hiện) thì với Streaming người dùng chỉ cần chờ đợi sinh ra Time to First Token (TTFT) (khoảng 2-3s) là có thể bắt đầu có chữ xuất hiện và đọc được rồi. Điều này tạo cảm giác mượt mà tức thời, đem lại trải nghiệm tốt cho người dùng. 
> Ngược lại, non-streaming phù hợp hơn trong các tác vụ như gọi API tự động ở server backend. Ví dụ các tác vụ chạy ngầm định kỳ ban đêm để generate báo cáo JSON hàng loạt. Bởi vì ở đây không cần tương tác với người dùng nên việc streaming là không cần thiết.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
