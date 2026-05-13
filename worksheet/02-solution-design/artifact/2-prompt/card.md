---
artifact: 2 — Lớp chỉ dẫn AI
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp chỉ dẫn AI

**Tình huống xử lý**: T-01 (Lỗi không chuyển tiếp khẩn cấp / Escalation Failure)  
Xem `../../1-map-and-format.md` Phần A.

---

## 1. Giải pháp là gì?

Thiết lập một rào chắn hệ thống (System Guardrail) mang tên **"Nguyên tắc ngắt lời khẩn cấp"**. Khi nhận diện các từ khóa chỉ sự hoảng loạn hoặc giới hạn thời gian gắt gao (ví dụ: "cứu", "sắp đóng gate", "lỡ chuyến", "tai nạn"), AI bị cấm tuyệt đối việc trích xuất (RAG) và giải thích các điều khoản chính sách. Thay vào đó, AI bắt buộc trả lời dưới 15 chữ và gọi hàm hệ thống để kích hoạt Handoff (chuyển người thật).

---

## 2. Vì sao sửa ở lớp chỉ dẫn AI?

- AI đang trả lời quá tự tin, ưu tiên việc "làm hài lòng bằng cách cung cấp thông tin" thay vì "cung cấp giải pháp hành động nhanh".
- AI cần luật rõ: khi nào giải thích, khi nào từ chối, khi nào bắt buộc chuyển sang người thật.
- Có thể sửa nhanh bằng prompt để đè lại (override) bản tính thích sinh văn bản dài của LLM, ngăn chặn độ trễ trong tình huống khẩn cấp.

**Hành động phòng vệ chính**:

- [x] Ngăn câu trả lời dài dòng/sai bối cảnh ngay từ đầu
- [ ] Bắt buộc nêu nguồn khi nói về thông tin quan trọng
- [ ] Từ chối trả lời khi thiếu căn cứ
- [x] Chuyển người thật khi vượt phạm vi (Vượt giới hạn thời gian xử lý)

---

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

Demo cần có:

- Luật System Prompt chính cho AI.
- Mẫu câu khi cần chuyển sang người thật khẩn cấp.
- 3 ví dụ hỏi đáp để kiểm tra luật (Khẩn cấp thật, Hỏi bình thường, Đùa cợt dùng từ khẩn cấp).
- Kết quả thử lại với các tình huống từ Bài 1.

---

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**

AI có thể trở nên "quá nhạy cảm" (False Positive), tự động ngắt chat và chuyển cho nhân viên thật ngay cả khi khách hàng chỉ dùng các từ lóng hoặc nói đùa (Ví dụ: "Cứu em với, giá vé đợt này cao quá"). Việc này gây quá tải cho hệ thống Live Agent.

**Nhóm giảm vấn đề đó bằng cách nào?**

Prompt sẽ yêu cầu AI đánh giá "Intent" (ý định) đi kèm "Context" (ngữ cảnh): Chỉ kích hoạt Emergency Mode khi có từ khóa khẩn cấp **cộng với** bối cảnh không gian/thời gian rõ ràng (đang ở sân bay, giờ bay sát nút). Với các trường hợp "than vãn", AI vẫn xử lý bình thường.

---

## 5. Checklist trước khi nộp

- [x] Luật viết đủ cụ thể để AI làm theo.
- [x] Có mẫu câu khi AI không có đủ thông tin / cần dừng lại.
- [x] Có ví dụ cho tình huống dễ sai (False positive).
- [x] Có thử lại bằng tình huống trong Bài 1.
- [x] Không dùng prompt như cách duy nhất (đã phối hợp với lớp UI/UX ở phần trước).

**Người phụ trách**: Chi & My