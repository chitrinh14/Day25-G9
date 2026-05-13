---
artifact: 3 — Lớp kiến trúc dữ liệu
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp kiến trúc dữ liệu

**Tình huống xử lý**: T-01 (Lỗi không chuyển tiếp khẩn cấp / Escalation Failure)  
Xem `../../1-map-and-format.md` Phần A.

---

## 1. Giải pháp là gì?

Xây dựng một **Bộ định tuyến phân loại khẩn cấp (Triage Router & Classifier API)** đặt ngay sau khi nhận câu hỏi của người dùng và TRƯỚC khi gọi hệ thống RAG. Nếu Classifier bắt được cờ đỏ (Red Flag - Khẩn cấp), hệ thống sẽ bỏ qua hoàn toàn luồng xử lý của AI, lập tức đẩy yêu cầu vào hệ thống CRM của nhân viên (Live Agent Queue) kèm mức độ Ưu tiên 1 (Priority 1).

---

## 2. Vì sao sửa ở lớp kiến trúc dữ liệu?

- Nguyên nhân chính là luồng AI (RAG + LLM generation) mất nhiều thời gian xử lý và có bản tính thích sinh văn bản dài để trả lời.
- Cần kiểm tra dữ liệu và phân loại (Triage) trước khi câu trả lời được LLM tạo ra để giảm độ trễ (latency) tối đa cho người dùng đang hoảng loạn.
- Việc đẩy thẳng ticket vào CRM ở mức kiến trúc sẽ đảm bảo nhân viên CSKH nhận được cảnh báo ngay lập tức.

**Hành động phòng vệ chính**:

- [ ] Ngăn lỗi bằng nguồn dữ liệu đúng
- [x] Phát hiện khi có rủi ro khẩn cấp tiềm ẩn (thông qua Classifier)
- [x] Khắc phục bằng cách chuyển sang người thật (Bypass LLM)
- [x] Ghi lại lỗi (False Positive/Negative) để cải thiện sau

---

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

Demo cần có:

- Sơ đồ luồng đi của dữ liệu qua Triage Router bằng ngôn ngữ Mermaid.
- Bảng mô tả các thành phần (Classifier, RAG, Routing Logic, CRM).
- Cách xử lý khi hệ thống gặp vấn đề (ví dụ: Classifier sập, hoặc tổng đài quá tải).

---

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**

Bộ Classifier có thể nhận diện nhầm (False Positive), ví dụ khách chỉ nói "cứu tôi, vé đắt quá" nhưng hệ thống lại tưởng là cấp cứu tại sân bay và đẩy sang nhân viên, làm tốn nguồn lực CSKH con người.

**Nhóm giảm vấn đề đó bằng cách nào?**

Sử dụng **Kiến trúc Lai (Hybrid Classifier)**: Không chỉ dựa vào từ khóa (Keywords) mà phải kết hợp với **Metadata** của mã đặt chỗ (PNR). Hệ thống chỉ bật cờ đỏ (Red Flag) nếu: Có từ khóa khẩn cấp + Giờ khởi hành của PNR là trong vòng 24 giờ tới. Nếu PNR bay vào tháng sau, hệ thống đẩy về luồng bot bình thường.

---

## 5. Checklist trước khi nộp

- [x] Sơ đồ cho thấy dữ liệu đi từ đâu đến đâu.
- [x] Có bước kiểm tra phân loại trước khi AI trả lời.
- [x] Có cách xử lý khi luồng dữ liệu khẩn cấp kích hoạt.
- [x] Có cách chuyển sang người thật với tình huống rủi ro cao.
- [x] Có cách biết lỗi này có đang lặp lại không.

**Người phụ trách**: Chi & My