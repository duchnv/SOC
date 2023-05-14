# Thành phần của Wazuh
## 1. Wazuh indexer

Wazuh indexer là một công cụ phân tích và tìm kiếm toàn văn bản, có khả năng mở rộng cao. Thành phần trung tâm Wazuh này lập chỉ mục và lưu trữ các cảnh báo do máy chủ Wazuh tạo ra và cung cấp khả năng tìm kiếm và phân tích dữ liệu gần thời gian thực. Bộ lập chỉ mục Wazuh có thể được cấu hình như một cụm một nút hoặc đa nút, cung cấp khả năng mở rộng và tính sẵn sàng cao.
   
Wazuh indexer lưu trữ dữ liệu dưới dạng tài liệu JSON. Mỗi tài liệu tương quan một tập hợp các khóa, tên trường hoặc thuộc tính, với các giá trị tương ứng của chúng có thể là chuỗi, số, boolean, ngày, mảng giá trị, vị trí địa lý hoặc các loại dữ liệu khác.
   
Indexer (Chỉ mục) là một tập hợp các tài liệu có liên quan với nhau. Các tài liệu được lưu trữ trong trình lập chỉ mục Wazuh được phân phối trên các vùng chứa khác nhau được gọi là phân đoạn. Bằng cách phân phối tài liệu trên nhiều phân đoạn và phân phối các phân đoạn đó trên nhiều nút, trình lập chỉ mục Wazuh có thể đảm bảo dự phòng. Điều này bảo vệ hệ thống của bạn chống lại các lỗi phần cứng và tăng khả năng truy vấn khi các nút được thêm vào một cụm.
   
Wazuh sử dụng bốn chỉ số khác nhau để lưu trữ các loại sự kiện khác nhau:
   
- **Wazuh‑alerts (Cảnh báo Wazuh):** Lưu trữ các cảnh báo được tạo bởi Wazuh server. Chúng được tạo mỗi khi sự kiện truy cập quy tắc có mức độ ưu tiên đủ cao (ngưỡng này có thể định cấu hình).
    
- **Wazuh‑archives (Wazuh-Lưu trữ):**:Lưu trữ tất cả các sự kiện (lưu trữ dữ liệu) mà Wazuh server nhận được, cho dù chúng có tuân theo quy tắc hay không.
    
- **Wazuh‑monitoring (Giám sát Wazuh):**	Lưu trữ dữ liệu liên quan đến trạng thái Wazuh agent theo thời gian. Nó được sử dụng bởi giao diện web để đại diện khi các tác nhân riêng lẻ đang hoặc đã được, hoặc. Active DisconnectedNever connected.
       
- **Wazuh‑statistics (Wazuh-Thống kê):**	Lưu trữ dữ liệu liên quan đến hiệu suất Wazuh server. Nó được sử dụng bởi giao diện web để đại diện cho số liệu thống kê hiệu suất.
 
![Wazuh indexer](/Images/wazuh-003.jpg)

## 2. Wazuh server

Wazuh server phân tích dữ liệu nhận được từ các tác nhân, kích hoạt cảnh báo khi phát hiện các mối đe dọa hoặc bất thường. Nó cũng được sử dụng để quản lý cấu hình tác nhân từ xa và theo dõi trạng thái của chúng.
   
Wazuh server sử dụng các nguồn thông tin về mối đe dọa để cải thiện khả năng phát hiện của nó. Nó cũng làm phong phú dữ liệu cảnh báo bằng cách sử dụng khuôn khổ MITRE ATT &CK và các yêu cầu tuân thủ quy định như PCI DSS, GDPR, HIPAA, CIS và NIST 800-53, cung cấp ngữ cảnh hữu ích cho các phân tích bảo mật.

Ngoài ra, Wazuh server có thể được tích hợp với phần mềm bên ngoài, bao gồm các hệ thống bán vé như ServiceNow, Jira và PagerDuty, cũng như các nền tảng nhắn tin tức thời như Slack. Những tích hợp này thuận tiện cho việc hợp lý hóa các hoạt động bảo mật.

**Kiến trúc máy chủ**

Wazuh server chạy công cụ phân tích, API Wazuh RESTful, dịch vụ đăng ký đại lý, dịch vụ kết nối tác nhân, trình nền cụm Wazuh và Filebeat. Máy chủ được cài đặt trên hệ điều hành Linux và thường chạy trên máy vật lý độc lập, máy ảo, bộ chứa docker hoặc phiên bản đám mây.
   
Sơ đồ dưới đây thể hiện kiến trúc máy chủ và các thành phần:
 
![Kiến trúc wazuh server](/Images/wazuh-004.png)

**Thành phần máy chủ**

Wazuh server bao gồm một số thành phần được liệt kê bên dưới có các chức năng khác nhau, chẳng hạn như đăng ký các đại lý mới, xác thực từng danh tính tác nhân và mã hóa thông tin liên lạc giữa Wazuh agent và Wazuh server.
   
- **Agent enrollment service (Dịch vụ đăng ký đại lý):** Nó được sử dụng để đăng ký các đại lý mới. Dịch vụ này cung cấp và phân phối các khóa xác thực duy nhất cho mỗi tác nhân. Quá trình này chạy như một dịch vụ mạng và hỗ trợ xác thực thông qua chứng chỉ TLS / SSL hoặc bằng cách cung cấp mật khẩu cố định.
    
- **Agent connection service (Dịch vụ kết nối đại lý):** Dịch vụ này nhận dữ liệu từ các đại lý. Nó sử dụng các khóa được chia sẻ bởi dịch vụ đăng ký để xác thực từng danh tính đại lý và mã hóa thông tin liên lạc giữa đại lý Wazuh và máy chủ Wazuh. Ngoài ra, dịch vụ này cung cấp quản lý cấu hình tập trung, cho phép bạn đẩy cài đặt tác nhân mới từ xa.
    
- **Analysis engine (Công cụ phân tích):** Đây là thành phần máy chủ thực hiện phân tích dữ liệu. Nó sử dụng bộ giải mã để xác định loại thông tin đang được xử lý (sự kiện Windows, nhật ký SSH, nhật ký máy chủ web và các thông tin khác). Các bộ giải mã này cũng trích xuất các yếu tố dữ liệu có liên quan từ các thông báo nhật ký, chẳng hạn như địa chỉ IP nguồn, ID sự kiện hoặc tên người dùng. Sau đó, bằng cách sử dụng các quy tắc, công cụ xác định các mẫu cụ thể trong các sự kiện được giải mã có thể kích hoạt cảnh báo và thậm chí có thể gọi các biện pháp đối phó tự động (ví dụ: cấm địa chỉ IP, dừng quá trình đang chạy hoặc xóa phần mềm độc hại).
    
- **Wazuh RESTful API** Dịch vụ này cung cấp một giao diện để tương tác với cơ sở hạ tầng Wazuh. Nó được sử dụng để quản lý cài đặt cấu hình của các tác nhân và máy chủ, theo dõi trạng thái cơ sở hạ tầng và sức khỏe tổng thể, quản lý và chỉnh sửa bộ giải mã và quy tắc Wazuh và truy vấn về trạng thái của các điểm cuối được giám sát. Bảng điều khiển Wazuh cũng sử dụng nó.
    
- **Analysis engine:** Dịch vụ này được sử dụng để mở rộng quy mô máy chủ Wazuh theo chiều ngang, triển khai chúng dưới dạng một cụm. Loại cấu hình này, kết hợp với bộ cân bằng tải mạng, cung cấp tính sẵn sàng và cân bằng tải cao. Daemon cụm Wazuh là những gì các máy chủ Wazuh sử dụng để giao tiếp với nhau và giữ cho đồng bộ hóa.
    
- **Filebeat (Tập tin):** Nó được sử dụng để gửi các sự kiện và cảnh báo đến trình lập chỉ mục Wazuh. Nó đọc đầu ra của công cụ phân tích Wazuh và vận chuyển các sự kiện trong thời gian thực. Nó cũng cung cấp cân bằng tải khi được kết nối với cụm chỉ mục Wazuh đa nút.

## 3. Wazuh dashboard

Wazuh dashboard (Bảng điều khiển Wazuh) là giao diện người dùng web linh hoạt và trực quan để khai thác, phân tích và trực quan hóa các sự kiện bảo mật và dữ liệu cảnh báo. Nó cũng được sử dụng để quản lý và giám sát nền tảng Wazuh. Ngoài ra, nó cung cấp các tính năng cho kiểm soát truy cập dựa trên vai trò (RBAC) và đăng nhập một lần (SSO).
   
**Data visualization and analysis (Trực quan hóa và phân tích dữ liệu)**

Giao diện web giúp người dùng điều hướng qua các loại dữ liệu khác nhau được thu thập bởi tác nhân Wazuh, cũng như các cảnh báo bảo mật do máy chủ Wazuh tạo ra. Người dùng cũng có thể tạo báo cáo và tạo trực quan hóa và bảng chỉ số tùy chỉnh.

**Agents monitoring and configuration (Giám sát và cấu hình tổng đài viên)**

Bảng điều khiển Wazuh cho phép người dùng quản lý cấu hình tác nhân và theo dõi trạng thái của họ. Ví dụ: đối với mỗi điểm cuối được giám sát, người dùng có thể xác định mô-đun tác nhân nào sẽ được bật, tệp nhật ký nào sẽ được đọc, tệp nào sẽ được theo dõi để thay đổi tính toàn vẹn hoặc kiểm tra cấu hình nào sẽ được thực hiện.

**Platform management (Quản lý nền tảng)**

Bảng điều khiển Wazuh cung cấp giao diện người dùng dành riêng để quản lý việc triển khai Wazuh của bạn. Điều này bao gồm giám sát trạng thái, nhật ký và thống kê của các thành phần Wazuh khác nhau. Nó cũng bao gồm cấu hình máy chủ Wazuh và tạo các quy tắc và bộ giải mã tùy chỉnh để phân tích nhật ký và phát hiện mối đe dọa.

**Developer tools (Công cụ dành cho nhà phát triển)**

Bảng điều khiển Wazuh bao gồm một công cụ Kiểm tra bộ quy tắc có thể xử lý các thông báo nhật ký để kiểm tra cách nó được giải mã và liệu nó có khớp với quy tắc phát hiện mối đe dọa hay không. Tính năng này đặc biệt hữu ích khi các bộ giải mã và quy tắc tùy chỉnh đã được tạo và người dùng muốn kiểm tra chúng.

## 4. Wazuh agent

Wazuh agent chạy trên Linux, Windows, macOS, Solaris, AIX và các hệ điều hành khác. Nó có thể được triển khai cho máy tính xách tay, máy tính để bàn, máy chủ, phiên bản đám mây, vùng chứa hoặc máy ảo. Tác nhân giúp bảo vệ hệ thống của bạn bằng cách cung cấp khả năng ngăn chặn, phát hiện và ứng phó mối đe dọa. Nó cũng được sử dụng để thu thập các loại dữ liệu hệ thống và ứng dụng khác nhau mà nó chuyển tiếp đến Wazuh server thông qua một kênh được mã hóa và xác thực.
   
**Agent architecture (Kiến trúc agent)**
   
Wazuh agent có kiến trúc mô-đun. Mỗi thành phần chịu trách nhiệm về các nhiệm vụ riêng của mình, bao gồm giám sát hệ thống tệp, đọc thông báo nhật ký, thu thập dữ liệu hàng tồn kho, quét cấu hình hệ thống và tìm kiếm phần mềm độc hại. Người dùng có thể quản lý các mô-đun tác nhân thông qua cài đặt cấu hình, điều chỉnh giải pháp cho các trường hợp sử dụng cụ thể của họ.
Sơ đồ dưới đây thể hiện kiến trúc tác nhân và các thành phần:
 
![Kiến trúc Agent](/Images/wazuh-005.png)

**Agent modules (Mô-đun đại lý)**
   
Tất cả các mô-đun tác nhân đều có thể cấu hình và thực hiện các tác vụ bảo mật khác nhau. Kiến trúc mô-đun này cho phép bạn bật hoặc tắt từng thành phần theo nhu cầu bảo mật của bạn. Dưới đây bạn có thể tìm hiểu về các mục đích khác nhau của tất cả các mô-đun tác nhân.
   
- **Log collector (Bộ thu nhật ký):** Thành phần tác nhân này có thể đọc các tệp nhật ký phẳng và các sự kiện Windows, thu thập thông báo nhật ký hệ điều hành và ứng dụng. Nó hỗ trợ các bộ lọc XPath cho các sự kiện Windows và nhận dạng các định dạng nhiều dòng như nhật ký kiểm tra Linux. Nó cũng có thể làm phong phú thêm các sự kiện JSON với siêu dữ liệu bổ sung.
    
- **Command execution (Thực hiện lệnh):** Các tổng đài viên chạy các lệnh được ủy quyền định kỳ, thu thập đầu ra của chúng và báo cáo lại cho máy chủ Wazuh để phân tích thêm. Bạn có thể sử dụng mô-đun này cho các mục đích khác nhau, chẳng hạn như theo dõi dung lượng đĩa cứng còn lại hoặc nhận danh sách người dùng đăng nhập cuối cùng.
    
- **File integrity monitoring (FIM) (Giám sát tính toàn vẹn của tệp):** Mô-đun này giám sát hệ thống tệp, báo cáo khi tệp được tạo, xóa hoặc sửa đổi. Nó theo dõi các thay đổi trong thuộc tính tệp, quyền, quyền sở hữu và nội dung. Khi một sự kiện xảy ra, nó nắm bắt chi tiết ai, cái gì và khi nào trong thời gian thực. Ngoài ra, mô-đun FIM xây dựng và duy trì cơ sở dữ liệu với trạng thái của các tệp được giám sát, cho phép các truy vấn được chạy từ xa.
    
- **Security configuration assessment (SCA) (Đánh giá cấu hình bảo mật):** Thành phần này cung cấp đánh giá cấu hình liên tục, sử dụng các kiểm tra có sẵn dựa trên các điểm chuẩn của Trung tâm Bảo mật Internet (CIS). Người dùng cũng có thể tạo kiểm tra SCA của riêng họ để giám sát và thực thi các chính sách bảo mật của họ.
    
- **System inventory (Kiểm kê hệ thống):** Mô-đun tác nhân này định kỳ chạy quét, thu thập dữ liệu hàng tồn kho như phiên bản hệ điều hành, giao diện mạng, quy trình đang chạy, ứng dụng đã cài đặt và danh sách các cổng mở. Kết quả quét được lưu trữ trong cơ sở dữ liệu SQLite cục bộ có thể được truy vấn từ xa.
    
- **Malware detection (Phát hiện phần mềm độc hại):** Sử dụng cách tiếp cận không dựa trên chữ ký, thành phần này có khả năng phát hiện sự bất thường và sự hiện diện có thể có của rootkit. Ngoài ra, nó tìm kiếm các quy trình ẩn, tệp ẩn và cổng ẩn trong khi giám sát các cuộc gọi hệ thống.
    
- **Active response (Phản hồi tích cực):** Mô-đun này chạy các hành động tự động khi phát hiện các mối đe dọa, kích hoạt phản hồi để chặn kết nối mạng, dừng quá trình đang chạy hoặc xóa tệp độc hại. Người dùng cũng có thể tạo phản hồi tùy chỉnh khi cần thiết và tùy chỉnh, ví dụ: phản hồi để chạy tệp nhị phân trong hộp cát, nắm bắt lưu lượng mạng và quét tệp bằng phần mềm chống vi-rút.
    
- **Container security monitoring (Giám sát an ninh container):** Mô-đun tác nhân này được tích hợp với API Docker Engine để giám sát các thay đổi trong môi trường được đóng gói. Ví dụ: nó phát hiện các thay đổi đối với hình ảnh vùng chứa, cấu hình mạng hoặc khối lượng dữ liệu. Bên cạnh đó, nó cảnh báo về các container đang chạy trong chế độ đặc quyền và về việc người dùng thực hiện các lệnh trong một container đang chạy.
    
- **Cloud security monitoring (Giám sát bảo mật đám mây):** Thành phần này giám sát các nhà cung cấp đám mây như Amazon AWS, Microsoft Azure hoặc Google GCP. Nó tự nhiên giao tiếp với API của họ. Nó có khả năng phát hiện các thay đổi đối với cơ sở hạ tầng đám mây (ví dụ: người dùng mới được tạo, nhóm bảo mật bị sửa đổi, phiên bản đám mây bị dừng, v.v.) và thu thập dữ liệu nhật ký dịch vụ đám mây (ví dụ: AWS Cloudtrail, AWS Macie, AWS GuardDuty, Azure Active Directory, v.v.)
    
**Giao tiếp với máy chủ Wazuh**
   
Wazuh agent (Tác nhân Wazuh) liên lạc với Wazuh server để gửi dữ liệu đã thu thập và các sự kiện liên quan đến bảo mật. Bên cạnh đó, tổng đài viên gửi dữ liệu hoạt động, báo cáo cấu hình và trạng thái của nó. Sau khi kết nối, tác nhân có thể được nâng cấp, giám sát và cấu hình từ xa từ máy chủ Wazuh.
   
Giao tiếp của tác nhân với máy chủ diễn ra thông qua một kênh an toàn (TCP hoặc UDP), cung cấp mã hóa và nén dữ liệu trong thời gian thực. Ngoài ra, nó bao gồm các cơ chế kiểm soát dòng chảy để tránh ngập lụt, các sự kiện xếp hàng khi cần thiết và bảo vệ băng thông mạng.
