# Kiến trúc và cơ chế hoạt động của Wazuh

Wazuh architecture (Kiến trúc Wazuh) dựa trên các agents, chạy trên các điểm cuối được giám sát, chuyển tiếp dữ liệu bảo mật đến một máy chủ trung tâm. Các thiết bị không cần tác nhân như tường lửa, thiết bị chuyển mạch, bộ định tuyến và điểm truy cập được hỗ trợ và có thể chủ động gửi dữ liệu nhật ký qua Syslog, SSH hoặc sử dụng API của chúng. Máy chủ trung tâm giải mã và phân tích thông tin đến và chuyển kết quả đến bộ chỉ mục Wazuh để lập chỉ mục và lưu trữ.

Wazuh indexer cluster (Cụm chỉ mục Wazuh) là một tập hợp gồm một hoặc nhiều nút giao tiếp với nhau để thực hiện các hoạt động đọc và ghi trên các chỉ số. Các triển khai Wazuh nhỏ, không yêu cầu xử lý lượng lớn dữ liệu, có thể dễ dàng được xử lý bởi một cụm nút đơn. Các cụm đa nút được khuyến nghị khi có nhiều điểm cuối được giám sát, khi dự đoán khối lượng dữ liệu lớn hoặc khi cần tính sẵn sàng cao.

Đối với môi trường sản xuất, bạn nên triển khai máy chủ Wazuh và trình lập chỉ mục Wazuh cho các máy chủ khác nhau. Trong trường hợp này, Filebeat được sử dụng để chuyển tiếp an toàn các cảnh báo Wazuh và các sự kiện lưu trữ đến cụm chỉ mục Wazuh (một nút hoặc đa nút) bằng cách sử dụng mã hóa TLS.

Sơ đồ dưới đây thể hiện kiến trúc triển khai Wazuh. 
Nó cho thấy các thành phần giải pháp và cách Wazuh server và các nút lập Wazuh indexer có thể được cấu hình thành các cụm, cung cấp cân bằng tải và tính sẵn sàng cao.
 
Hình 6. Kiến trúc triển khai

**Wazuh agent - Wazuh server communication**

Wazuh agent liên tục gửi các sự kiện đến Wazuh server để phân tích và phát hiện mối đe dọa. Để bắt đầu vận chuyển dữ liệu này, tác nhân thiết lập kết nối với dịch vụ máy chủ để kết nối tác nhân, dịch vụ này lắng nghe trên cổng 1514 theo mặc định (điều này có thể định cấu hình). Máy chủ Wazuh sau đó giải mã và kiểm tra quy tắc các sự kiện nhận được, sử dụng công cụ phân tích. Các sự kiện làm hỏng quy tắc được tăng cường với dữ liệu cảnh báo như ID quy tắc và tên quy tắc. Các sự kiện có thể được cuộn vào một hoặc cả hai tệp sau, tùy thuộc vào việc quy tắc có bị vấp hay không:

- Tệp chứa tất cả các sự kiện cho dù chúng có vấp quy tắc hay không. /var/ossec/logs/archives/archives.json

- Tệp chỉ chứa các sự kiện vấp quy tắc với mức độ ưu tiên đủ cao (ngưỡng có thể định cấu hình). /var/ossec/logs/alerts/alerts.json

**Wazuh server - Wazuh indexer communication**

Wazuh server sử dụng Filebeat để gửi dữ liệu cảnh báo và sự kiện đến trình lập chỉ mục Wazuh, sử dụng mã hóa TLS. Filebeat đọc dữ liệu đầu ra của máy chủ Wazuh và gửi nó đến bộ chỉ mục Wazuh (theo mặc định nghe trên cổng 9200 / TCP). Khi dữ liệu được lập chỉ mục bởi trình lập chỉ mục Wazuh, bảng điều khiển Wazuh được sử dụng để khai thác và trực quan hóa thông tin.

Bảng điều khiển Wazuh truy vấn API Wazuh RESTful (theo mặc định lắng nghe trên cổng 55000/TCP trên máy chủ Wazuh) để hiển thị thông tin liên quan đến cấu hình và trạng thái của Wazuh server (Máy chủ Wazuh) và các agent. Nó cũng có thể sửa đổi các tác nhân hoặc cài đặt cấu hình máy chủ thông qua các lệnh gọi API. Giao tiếp này được mã hóa bằng TLS và được xác thực bằng tên người dùng và mật khẩu.
