# 1. Giới thiệu về Sysmon

Sysmon (System Monitor) là bộ công cụ của Microsoft dùng để kiểm tra những hoạt động của Windows gồm: tiến trình, kết nối mạng, file, … và đưa vào Windows Event Log. 

Thông tin được ghi lại bao gồm quy trình khởi tạo tiến trình, kết nối mạng hoặc sự thay đổi của các file trong hệ thống. 

Từ đó, các kỹ sư bảo mật có thể thu nhập thông từ event log sau đó đẩy về các hệ thống SIEM để tiến hình phân tích, xác định các hành vi của phần mềm độc hại trên hệ thống công nghệ thông tin của mình.

# 2. Chức năng của Sysmon

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

# 3. Một số sự kiện thường gặp trong Sysmon

![sysmon-001](/Images/sysmon-001.png)

![sysmon-002](/Images/sysmon-002.png)

![sysmon-003](/Images/sysmon-003.png)

# 4. Ưu và nhược điểm của Sysmon

**Ưu điểm**

- Dễ dàng tích hợp trên nền tảng Windows.

- Cho phép ghi lại logs chứa mã băm của tiến trình được thực thi.

- Cung cấp miễn phí bởi Microsoft.

- Có khả năng hỗ trợ phát hiện mã độc mà các logs mặc định không làm được.

-Ghi lại logs liên quan đến các tiến trình nạp driver, DLL, thay đổi khóa registry, …

- Ghi lại logs liên quan đến các kết nối mạng.

**Nhược điểm**

- Khả năng gây nhiễu, sinh nhiều logs nên cần lọc, theo dõi và tối ưu trong quá trình triển khai giám sát.

-Kẻ tấn công có thể dễ dàng vô hiệu hóa, vượt qua hoặc phá hoại dịch vụ Sysmon nên cần giám sát sự thay đổi cấu hình dịch vụ

