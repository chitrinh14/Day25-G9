---
artifact: 1 — Demo giao diện
format: phác thảo ASCII
---

# demo.md — Demo giao diện

File này phác thảo giao diện ứng dụng trên điện thoại, tập trung vào tính năng "Triage & Handoff" (Phân loại cấp cứu & Chuyển giao) cho rủi ro T-01.

---

## 1. Màn hình chính (ASCII Mockup)

### Màn hình A: Trạng thái bình thường (Người dùng bắt đầu chat)

```text
+-------------------------------------------+
| < Trở lại     Trợ lý AI Hàng không  [SOS] |  <-- Nút SOS luôn hiện diện (Màu đỏ)
+-------------------------------------------+
| AI: Chào bạn, tôi là trợ lý ảo. Bạn       |
| cần tra cứu thông tin gì hôm nay?         |
|                                           |
| User: Cứu em với, em đang kẹt ở soi chiếu |
| an ninh sân bay TSN, chuyến VN123 báo 10  |
| phút nữa đóng gate!!!                     |
|                                           |
|                                           |
|                                           |
|                                           |
+-------------------------------------------+
| [ Nhập tin nhắn...                      ] | 
+-------------------------------------------+
```

### Màn hình B: Kích hoạt Hỗ trợ khẩn cấp (Handoff)
Giao diện thay đổi ngay lập tức khi hệ thống phát hiện từ khóa khẩn cấp hoặc người dùng bấm nút SOS.

```text
+------------------------------------------+
| < Trở lại     Hỗ trợ Khẩn cấp      [SOS] |  <-- SOS nháy sáng
+------------------------------------------+
| ---------------------------------------- |
| 🚨 HỆ THỐNG PHÁT HIỆN TÌNH HUỐNG GẤP     |  <-- Banner cảnh báo đỏ
|                                          |
| AI: Tôi đã ghi nhận tình huống của bạn   |
| và đang kết nối ngay với nhân viên trực  |
| sân bay. Vui lòng giữ máy.               |
|                                          |
| [ ĐANG KẾT NỐI NHÂN VIÊN... 80% ]        |  <-- Thanh trạng thái
|                                          |
| Vị trí chờ của bạn: #01                  |
+------------------------------------------+
| [ 🔒 Khung chat tạm khóa để xử lý ]      |  <-- Khung nhập liệu bị khóa
+------------------------------------------+
```

---

## 2. Trạng thái cần minh họa

| Trạng thái | Người dùng thấy gì? | Người dùng làm gì tiếp? |
|---|---|---|
| **Bình thường** | Nút SOS màu đỏ nhạt ở góc phải Header. AI trả lời như bình thường. | Nhập câu hỏi tra cứu thông tin chung. |
| **Phát hiện khẩn cấp** | Banner đỏ 🚨 xuất hiện. Nút SOS nháy sáng. AI thông báo ngừng trả lời để chuyển máy. | Đọc thông báo, cảm thấy yên tâm vì yêu cầu đã được nâng biên. |
| **Chuyển người thật** | Khung chat bị khóa mờ (Grey out). Hiển thị số thứ tự chờ nhân viên CSKH. | Chờ nhân viên thật vào tiếp quản và xử lý trực tiếp. |

---

## 3. Ghi chú cho từng thành phần

- **Nút [SOS]**: Đặt tại Header để luôn hiển thị dù người dùng cuộn tin nhắn xuống dưới. Đây là lớp chặn cuối nếu AI không tự nhận diện được độ khẩn cấp.
- **Banner 🚨**: Sử dụng màu đỏ cảnh báo để tạo sự chú ý tức thì, giúp người dùng biết hệ thống đã "hiểu" vấn đề của họ.
- **Khóa khung chat (🔒)**: Rất quan trọng để ngăn AI tiếp tục đưa ra các thông tin chính sách gây nhiễu trong lúc khách đang vội.

---

## 4. Kiểm tra nhanh

- [x] Chặn được rủi ro AI trả lời dài dòng khi khách sắp trễ chuyến.
- [x] Có nút bypass chủ động (SOS).
- [x] Có thông báo chuyển tiếp sang người thật rõ ràng.
- [x] Câu chữ ngắn gọn, mang tính định hướng hành động.