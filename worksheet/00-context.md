# 00-context.md — Bối cảnh sản phẩm của nhóm

## 1. Sản phẩm

* **Tên sản phẩm / bot**: Trợ lý đặt vé và chăm sóc khách hàng hàng không (AI Flight Assistant).
* **Sản phẩm giúp ai làm gì**: Trích xuất và giải đáp thông tin từ cơ sở dữ liệu (FAQ/Policy) về quy định hành lý, điều kiện hạng vé, thủ tục hoàn/đổi và tình trạng chuyến bay thời gian thực.
* **Người dùng gặp sản phẩm ở đâu**: Website chính thức và ứng dụng di động của hãng hàng không.
* **Giai đoạn hiện tại**: Đang thử nghiệm và xây dựng kịch bản kiểm thử an toàn (Eval Plan).

## 2. Phạm vi

**AI được làm gì**

* Giải đáp quy định hành lý, điều kiện hạng vé (Fare Rules).
* Hướng dẫn thủ tục hoàn/đổi vé dựa trên chính sách của hãng.
* Tra cứu tình trạng chuyến bay thời gian thực.
* Đóng vai trò lớp truy xuất thông tin (Retrieval) và gợi ý (Recommendation).

**AI không được làm gì**

* Không trực tiếp thực hiện giao dịch thanh toán.
* Không tự ý thay đổi lịch trình trên hệ thống booking.
* Không có quyền ghi dữ liệu trực tiếp vào hệ thống của hãng.
* Không tự ý cam kết các trường hợp ngoại lệ (như hoàn tiền cho vé không được phép hoàn) mà không có thẩm quyền.

**Vì sao có giới hạn này**

* Tránh rủi ro pháp lý liên quan đến cam kết tài chính sai lệch.
* Đảm bảo an toàn dữ liệu hệ thống đặt chỗ và tránh gây thiệt hại tài chính trực tiếp cho cả khách hàng lẫn hãng hàng không.

## 3. Người dùng

* **Là ai**: Hành khách của hãng hàng không (đa dạng trình độ), thường là người đã có mã đặt chỗ (PNR) hoặc đang tìm kiếm chuyến bay.
* **Họ hỏi AI khi nào**: Khi cần quyết định nhanh về dịch vụ, khi gặp sự cố lịch trình (delay/thay đổi) hoặc có nhu cầu khiếu nại.
* **Họ cần quyết định gì sau khi hỏi AI**: Có nên thực hiện hoàn/đổi vé hay không, chuẩn bị hành lý/giấy tờ như thế nào để không bị từ chối bay.
* **Khi nào họ dễ bị tổn thương / dễ hiểu sai**: Khi đang lo lắng do chuyến bay bị hủy/đổi giờ, hoặc khi hỏi về các hạng vé hạn chế (như Economy Lite) vốn có chính sách khắt khe.
* **Họ thường tin AI đến mức nào**: Rất tin tưởng, coi phản hồi của AI là thông tin chính thức tương đương nhân viên CSKH (High trust).

## 4. Bối cảnh ngành

* **Sự cố tương tự đã từng xảy ra**: Case chatbot NEO của Vietnam Airlines thất bại trong việc tối ưu trải nghiệm người dùng.
* **Quy định hoặc ràng buộc liên quan**: Chính sách giá vé (Fare Rules), quy định về bồi thường hàng không và các tiêu chuẩn an toàn bay.
* **Nguồn chính thức nên ưu tiên**: Cơ sở dữ liệu chính sách (FAQ), bảng điều kiện giá vé của hãng hàng không.