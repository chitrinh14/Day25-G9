---
artifact: 1 — FINAL kế hoạch giải pháp
bai-tap: 2 — Thiết kế giải pháp
phase: Chọn rủi ro + chọn tầng + chọn demo + chốt 3 lớp giải pháp
time: 11:00-11:55
input: 00-context.md + 01-test-set-review/3-FINAL-test-set-eval-plan.md
nop-cuoi: Có — file cuối Bài 2
---

# 1 — FINAL: Kế hoạch giải pháp

File này ghi lại quyết định chính của Bài 2:

- Rủi ro nào được chọn.
- Vì sao rủi ro đó quan trọng.
- Nguyên nhân gốc là gì.
- Nhóm sẽ xây 3 lớp giải pháp nào.
- Mỗi lớp dùng demo gì.

Lý do cần 3 lớp: một giải pháp đơn lẻ dễ lọt lỗi. Với rủi ro nặng, nhóm cần nhiều lớp cùng đỡ: lớp này ngăn, lớp kia phát hiện, lớp khác khắc phục hoặc thông báo cho người dùng.

Ba lớp giải pháp nằm trong thư mục `artifact/`:

| Lớp | Thư mục | Vai trò |
|---|---|---|
| Giao diện | `artifact/1-uiux/` | Cảnh báo, dẫn nguồn, nút chuyển sang người thật |
| Chỉ dẫn AI | `artifact/2-prompt/` | Hỏi lại, từ chối, bắt buộc dẫn nguồn |
| Kiến trúc dữ liệu | `artifact/3-architecture/` | Tra cứu nguồn đúng, lưu tạm dữ liệu, xử lý khi thiếu nguồn, giám sát |

Ba lớp này bổ sung cho nhau. Nếu một lớp lọt lỗi, lớp khác vẫn có thể chặn hoặc giảm hại.

## Thông tin nhóm

- **Chủ đề**: Trợ lý đặt vé và chăm sóc khách hàng hàng không (AI Flight Assistant).
- **Thành viên**: Chi, My
- **Ngày**: 2026-05-13

---

## Phần A — Chọn rủi ro và tầng giải pháp

### Rủi ro chính được chọn

- **ID tình huống**: T-01
- **Mô tả ngắn**: Khi hành khách đang gặp tình huống khẩn cấp tại sân bay (như sắp đóng gate, lỡ chuyến, trẻ nhỏ ốm), AI có xu hướng trả lời rập khuôn, trích dẫn luật lệ dài dòng thay vì cung cấp giải pháp hành động ngay lập tức, gây lỡ chuyến bay và trải nghiệm tồi tệ cho người dùng.
- **Mức độ**: Nặng (MUST fix)
- **Điểm rủi ro**: 25/25 (Impact 5 x Urgency 5)
- **Vì sao chọn tình huống này**: Đây là điểm mù nguy hiểm nhất của một Chatbot CSKH. Hành khách ở trạng thái này rất dễ kích động (panic). Nếu AI không thể Escalation (chuyển tiếp) kịp thời, hãng sẽ đối mặt trực tiếp với thiệt hại tài chính (đền bù vé) và khủng hoảng truyền thông tại sân bay.

### Tìm nguyên nhân gốc

Đừng chỉ mô tả lỗi. Hãy trả lời: vì sao lỗi xảy ra?

- [ ] Thiếu nguồn dữ liệu đúng.
- [ ] AI đoán khi không biết.
- [ ] Giao diện khiến người dùng tin quá mức.
- [x] Quy trình thiếu người duyệt hoặc thiếu bước chuyển sang người thật (Escalation failure).
- [ ] Không có theo dõi sau khi ra mắt.
- [x] Khác: AI không được lập trình để phân tích sắc thái cảm xúc/mức độ khẩn cấp (Urgency/Sentiment Classification) của đầu vào, dẫn đến việc xử lý mọi query như thông tin tra cứu bình thường.

### Bảng nối nguyên nhân với tầng sửa

| Nguyên nhân gốc | Tầng ưu tiên sửa | Lớp giải pháp liên quan |
|---|---|---|
| Thiếu nguồn đúng | Dữ liệu / tra cứu nguồn (RAG) / chính sách nguồn | `3-architecture` là chính |
| AI đoán bừa | Chỉ dẫn hệ thống / quy tắc từ chối / dẫn nguồn | `2-prompt` là chính |
| Người dùng tin quá mức | Giao diện cảnh báo / cách viết mức tin cậy | `1-uiux` là chính |
| Tình huống nhạy cảm (Khẩn cấp/Panic) | Phân loại ý định / Chuyển sang người thật | `1-uiux` + `2-prompt` + `3-architecture` |

### Kết luận Phần A

**Nguyên nhân gốc**: Hệ thống hiện tại thiếu cơ chế "Triage" (phân loại mức độ khẩn cấp). Nó đối xử với câu hỏi "Tôi lỡ chuyến, cửa sắp đóng" giống hệt câu hỏi "Hành lý 20kg giá bao nhiêu?".

**Tầng chính cần sửa**: Quy trình xử lý (Escalation routing) và Chỉ dẫn AI (Role boundary).

**Vì sao cần 3 lớp giải pháp**:
- **Lớp giao diện (`1-uiux`)**: Cần thiết kế nút "SOS / Hỗ trợ Khẩn cấp tại sân bay" luôn hiển thị để Bypass AI khi cần, đồng thời có UI thông báo rõ AI đang chuyển máy cho nhân viên thật.
- **Lớp chỉ dẫn AI (`2-prompt`)**: Cần System Prompt cứng yêu cầu AI nhận diện các keyword khẩn cấp (gate, lỡ, muộn, cấp cứu) để ngắt luồng sinh text dài và xuất ra trigger chuyển hướng.
- **Lớp kiến trúc dữ liệu (`3-architecture`)**: Cần một lớp phân loại (Classifier API) đặt trước RAG. Nếu Classifier đánh dấu là "High Urgency", hệ thống không tốn thời gian gọi RAG nữa mà định tuyến thẳng sang Live Agent Queue.

---

## Phần B — Chọn định dạng demo

Mỗi lớp cần một bản demo. Demo giúp biến ý tưởng thành thứ trực quan để nhóm khác xem, kiểm tra và phản biện.

| Lớp | Thư mục | Định dạng demo chọn | Thời gian dự kiến |
|---|---|---|---|
| Giao diện | `1-uiux` | ASCII UI mockup | 15 phút |
| Chỉ dẫn AI | `2-prompt` | Bản prompt trong Markdown + ví dụ | 15 phút |
| Kiến trúc dữ liệu | `3-architecture` | Sơ đồ Mermaid (Flowchart) | 20 phút |

**Lý do chọn demo**

- **Giao diện**: ASCII là cách nhanh nhất để team hình dung vị trí nút bấm và thông báo chuyển hướng trên màn hình chat mà không cần tool thiết kế.
- **Chỉ dẫn AI**: Prompt Markdown cho phép nhìn rõ Guardrails và điều kiện If/Then mà mô hình phải tuân theo.
- **Kiến trúc dữ liệu**: Mermaid sinh ra biểu đồ luồng (Flowchart) rất rõ ràng để thấy dữ liệu đi qua Classifier trước khi quyết định gọi LLM hay đẩy cho Human.

---

## Phần C — Ba lớp giải pháp

### Lớp 1 — Giao diện (`artifact/1-uiux/`)

- **Cách tiếp cận**: Thêm nút "Hỗ trợ Khẩn cấp" (Panic Button) cố định trên khung chat. Khi AI nhận diện tình huống khẩn cấp, UI tự động khóa khung nhập liệu và hiển thị Countdown chờ nhân viên thật.
- **Hành động phòng vệ bao phủ**: Khắc phục / Thông báo
- **Demo**: Bản vẽ ASCII UI.
- **Trạng thái**: Đang làm

Link chi tiết:
- `artifact/1-uiux/card.md`
- `artifact/1-uiux/demo.txt`

### Lớp 2 — Chỉ dẫn AI (`artifact/2-prompt/`)

- **Cách tiếp cận**: Bổ sung Guardrail: "Nguyên tắc ngắt lời". Yêu cầu AI không giải thích chính sách khi phát hiện intent khẩn cấp. Trả lời dưới 15 chữ và gọi hàm `escalate_to_human()`.
- **Hành động phòng vệ bao phủ**: Ngăn / Khắc phục
- **Demo**: System Prompt Markdown với Few-shot examples.
- **Trạng thái**: Đang làm

Link chi tiết:
- `artifact/2-prompt/card.md`
- `artifact/2-prompt/demo.md`

### Lớp 3 — Kiến trúc dữ liệu (`artifact/3-architecture/`)

- **Cách tiếp cận**: Xây dựng một Triage Router. Trước khi Prompt vào LLM, nó đi qua một Sentiment/Keyword Classifier siêu nhẹ. Nếu Flag = Red (Khẩn cấp), Router chặn luồng RAG và đẩy thẳng Ticket lên CRM của Human Agent kèm mức độ Priority 1.
- **Hành động phòng vệ bao phủ**: Phát hiện / Khắc phục
- **Demo**: Mermaid Flowchart.
- **Trạng thái**: Đang làm

Link chi tiết:
- `artifact/3-architecture/card.md`
- `artifact/3-architecture/demo.md`

---

## Tổng kiểm tra

| Câu hỏi | Trả lời |
|---|---|
| Rủi ro chính đã chọn là gì? | T-01 (Escalation Failure) |
| Nguyên nhân gốc là gì? | Thiếu cơ chế phân loại (Triage) mức độ khẩn cấp và thiếu luồng chuyển giao (handoff). |
| 3 lớp giải pháp đã đủ chưa? | Giao diện: Xong định hướng / Chỉ dẫn AI: Xong định hướng / Kiến trúc: Xong định hướng |
| 4 hành động đã bao phủ chưa? | Ngăn: Có (Prompt) / Phát hiện: Có (Kiến trúc) / Khắc phục: Có (Kiến trúc+UI) / Thông báo: Có (UI) |
| Nhóm khác đã góp ý chưa? | Đang đợi review. |
| Nhóm đã sửa gì sau phản biện? | Sẽ cập nhật. |