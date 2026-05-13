---
artifact: 1 — Lớp giao diện
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp giao diện

**Tình huống xử lý**: T-01 (Lỗi không chuyển tiếp trong tình huống khẩn cấp tại sân bay)  
Xem `../../1-map-and-format.md` Phần A.

---

## 1. Giải pháp là gì?

Giao diện Chatbot được bổ sung một nút **"SOS / Khẩn cấp"** luôn hiển thị ở góc trên màn hình. Khi nút này được bấm (do người dùng chủ động) HOẶC khi hệ thống tự động phát hiện intent khẩn cấp (sắp đóng gate, tai nạn, lỡ chuyến), giao diện sẽ ngay lập tức **khóa khung nhập liệu** (để ngăn AI sinh text dài dòng vô ích) và hiển thị bảng đếm ngược kết nối với Nhân viên CSKH thật.

---

## 2. Vì sao sửa ở lớp giao diện?

- Rủi ro xảy ra ở khoảnh khắc người dùng đọc câu trả lời: Trong lúc hoảng loạn ở sân bay, hành khách không có thời gian đọc một đoạn văn dài 300 chữ về luật hàng không do AI sinh ra.
- Nếu prompt hoặc dữ liệu (Classifier) vẫn sót lỗi không nhận ra mức độ khẩn cấp, nút SOS trên giao diện là lớp chặn cuối cùng để hành khách tự cứu mình bằng cách bypass (bỏ qua) con bot.

**Hành động phòng vệ chính**:

- [x] Thông báo rõ giới hạn (AI thông báo dừng trả lời)
- [ ] Phát hiện dấu hiệu thiếu nguồn
- [x] Chuyển người thật khi cần (Handoff)
- [ ] Giúp người dùng kiểm tra lại nguồn

---

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

**Định dạng demo**:

- [x] Phác thảo màn hình (ASCII Mockup)
- [x] Luồng màn hình
- [ ] Bản HTML đơn giản
- [ ] Ảnh hoặc link prototype

**Thành phần cần có trong demo**:
- Trạng thái trò chuyện bình thường (Có nút SOS).
- Trạng thái Handoff (AI bị khóa, chuyển sang người thật).
- Câu chữ cảnh báo ngắn, mang tính xoa dịu.

---

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**
Khách hàng có thể lạm dụng (spam) nút SOS ngay cả khi chỉ muốn hỏi thông tin bình thường (để lách luật không phải nói chuyện với bot), dẫn đến quá tải đội ngũ nhân viên CSKH con người (Human Agents).

**Nhóm giảm vấn đề đó bằng cách nào?**
Chỉ hiển thị nút SOS "Sáng lên" khi GPS định vị người dùng đang ở khu vực Sân bay, hoặc thêm một popup xác nhận siêu nhanh: *"Tình huống này liên quan đến chuyến bay trong 2 giờ tới? [Đúng, gọi nhân viên] - [Không, tôi hỏi thường]"* để lọc bớt các ca spam.

---

## 5. Checklist trước khi nộp

- [x] Giải pháp gắn đúng với một rủi ro chính.
- [x] Demo nhìn vào là hiểu vấn đề được chặn ở đâu.
- [x] Có đủ trạng thái bình thường và trạng thái lỗi.
- [x] Có cách chuyển sang người thật khi AI không nên tự xử lý.
- [x] Câu chữ trong giao diện ngắn, không đổ hết trách nhiệm cho người dùng.

**Người phụ trách**: Chi & My