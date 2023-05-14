# Chức năng của Sysmon

Một số khả năng của Sysmon:

- Ghi nhật ký quá trình tạo tiến trình với đầy đủ dạng lệnh cho cả tiến trình hiện tại và tiến trình tra.

- Ghi lại giá trị băm của tập tin hình ảnh khởi tạo tiến trình với giá trị mặc định là SHA1, ngồi ra còn các giá trị khác như MD5, SHA256 hoặc IMPHASH.

- Nhiều hàm băm có thể được sử dụng cùng một lúc.

- Lưu trữ giá trị GUID trong quá trình tạo sự kiện để cho phép tổng hợp, phân tích sự tương quan giữa các sự kiện ngay cả khi hệ điều hành Windows sử dụng lại PID.

- Giá trị Session GUID trong mỗi sự kiện để cho phép tổng hợp, phân tích tương quan các sự kiện trên cùng một phiên đăng nhập.

- Nhật ký quá trình tải, quá trình điều khiển hệ thống, thư viện với chữ ký và giá trị băm của từng tập tin.

- Lưu nhật ký mở, đọc ổ đĩa.

- Lưu nhật ký kết nối mạng, bao gồm thông tin tiến trình kết nối, địa chỉ IP, cổng, địa chỉ kết nối.

- Phát hiện các thay đổi về thời gian tạo tập tin để thu thông tin liên quan tới sự kiện thay đổi tập tin. Thay đổi thời gian tạo tập tin là một trong những kỹ thuật hay được mã độc sử dụng.

- Tự động tải lại cấu hình nếu có thay đổi trong Registry.

- Cơ chế lọc giúp loại trừ các sự kiện một cách linh hoạt.

- Theo dõi, giám sát hệ thống ngay từ khi quá trình khởi động để nắm bắt được toàn bộ hoạt động của các tiến trình trên hệ thống.

