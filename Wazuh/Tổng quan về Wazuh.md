# Giới thiệu về Wazuh
Giải pháp Wazuh bao gồm các tác nhân bảo mật, được triển khai trên các điểm cuối được giám sát và các thành phần trung tâm Wazuh, thu thập và phân tích dữ liệu được thu thập bởi các tác nhân.
Sơ đồ dưới đây minh họa kiến trúc của môi trường phòng thí nghiệm Wazuh được yêu cầu để kiểm tra các trường hợp sử dụng được mô tả trong tài liệu này.

Hình 1. Kiến trúc môi trường phòng thí nghiệm Wazuh

Nền tảng Wazuh cung cấp các tính năng XDR và SIEM để bảo vệ khối lượng công việc đám mây, vùng chứa và máy chủ của bạn. Chúng bao gồm phân tích dữ liệu nhật ký, xâm nhập và phát hiện phần mềm độc hại, giám sát tính toàn vẹn của tệp, đánh giá cấu hình, phát hiện lỗ hổng và hỗ trợ tuân thủ quy định.
Giải pháp Wazuh dựa trên tác nhân Wazuh, được triển khai trên các điểm cuối được giám sát và trên ba thành phần trung tâm: Wazuh server, Wazuh indexer và Wazuh dashboard.
•	Wazuh indexer là một công cụ phân tích và tìm kiếm toàn văn bản, có khả năng mở rộng cao. Thành phần trung tâm này lập chỉ mục và lưu trữ các cảnh báo được tạo bởi máy chủ Wazuh.
•	Wazuh server phân tích dữ liệu nhận được từ các tác nhân. Nó xử lý nó thông qua các bộ giải mã và quy tắc, sử dụng thông tin về mối đe dọa để tìm kiếm các chỉ số thỏa hiệp nổi tiếng (IOC). Một máy chủ duy nhất có thể phân tích dữ liệu từ hàng trăm hoặc hàng nghìn tác nhân và mở rộng quy mô theo chiều ngang khi được thiết lập dưới dạng một cụm. Thành phần trung tâm này cũng được sử dụng để quản lý các tác nhân, cấu hình và nâng cấp chúng từ xa khi cần thiết.
•	Wazuh dashboard là giao diện người dùng web để trực quan hóa và phân tích dữ liệu. Nó bao gồm các bảng điều khiển có sẵn cho các sự kiện bảo mật, tuân thủ quy định (ví dụ: PCI DSS, GDPR, CIS, HIPAA, NIST 800-53), các ứng dụng dễ bị tổn thương được phát hiện, dữ liệu giám sát tính toàn vẹn của tệp, kết quả đánh giá cấu hình, sự kiện giám sát cơ sở hạ tầng đám mây và các sự kiện khác. Nó cũng được sử dụng để quản lý cấu hình Wazuh và theo dõi trạng thái của nó.
•	Wazuh agents được cài đặt trên các điểm cuối như máy tính xách tay, máy tính để bàn, máy chủ, phiên bản đám mây hoặc máy ảo. Chúng cung cấp khả năng ngăn ngừa, phát hiện và phản hồi mối đe dọa. Chúng chạy trên các hệ điều hành như Linux, Windows, macOS, Solaris, AIX và HP-UX.
Ngoài khả năng giám sát dựa trên tác nhân, nền tảng Wazuh có thể giám sát các thiết bị không có tác nhân như tường lửa, thiết bị chuyển mạch, bộ định tuyến hoặc ID mạng, trong số những thiết bị khác. Ví dụ: dữ liệu nhật ký hệ thống có thể được thu thập thông qua Syslog và cấu hình của nó có thể được theo dõi thông qua thăm dò định kỳ dữ liệu của nó, thông qua SSH hoặc thông qua API.
Sơ đồ dưới đây thể hiện các thành phần và luồng dữ liệu của Wazuh.
 
Hình 2. Các thành phần và luồng dữ liệu của Wazuh
