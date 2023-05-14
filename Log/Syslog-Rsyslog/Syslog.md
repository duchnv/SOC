# Tổng quan về Syslog

## 1. Giới thiệu về Syslog

Syslog là một giao thức client/server là giao thức dùng để chuyển log và thông điệp đến máy nhận log. Máy nhận log thường được gọi là syslogd, syslog daemon hoặc syslog server. Syslog có thể gửi qua UDP hoặc TCP. Các dữ liệu được gửi dạng cleartext. Syslog dùng cổng 514.

Syslog được phát triển năm 1980 bởi Eric Allman, nó là một phần của dự án Sendmail, và ban đầu chỉ được sử dụng duy nhất cho Sendmail. Nó đã thể hiện giá trị của mình và các ứng dụng khác cũng bắt đầu sử dụng nó. Syslog hiện nay trở thành giải pháp khai thác log tiêu chuẩn trên Unix-Linux cũng như trên hàng loạt các hệ điều hành khác và thường được tìm thấy trong các thiết bị mạng như router Trong năm 2009, Internet Engineering Task Forec (IETF) đưa ra chuẩn syslog trong RFC 5424.

Syslog ban đầu sử dụng UDP, điều này là không đảm bảo cho việc truyền tin. Tuy nhiên sau đó IETF đã ban hành RFC 3195 (Đảm bảo tin cậy cho syslog) và RFC 6587 (Truyền tải thông báo syslog qua TCP). Điều này có nghĩa là ngoài UDP thì giờ đây syslog cũng đã sử dụng TCP để đảm bảo an toàn cho quá trình truyền tin.

Trong chuẩn syslog, mỗi thông báo đều được dán nhãn và được gán các mức độ nghiêm trọng khác nhau. Các loại phần mềm sau có thể sinh ra thông báo: auth, authPriv, daemon, cron, ftp, dhcp, kern, mail, syslog, user,... Với các mức độ nghiêm trọng từ cao nhất trở xuống Emergency, Alert, Critical, Error, Warning, Notice, Info, and Debug.

## 2. Mục đích của Syslog

Syslog được sử dụng như một tiêu chuẩn, chuyển tiếp và thu thập log được sử dụng trên một phiên bản Linux. Syslog xác định mức độ nghiêm trọng (severity levels) cũng như mức độ cơ sở (facility levels) giúp người dùng hiểu rõ hơn về nhật ký được sinh ra trên máy tính của họ. Log (nhật ký) có thể được phân tích và hiển thị trên các máy chủ được gọi là máy chủ Syslog.

Giao thức syslog có những yếu tố sau:

- Defining an architecture (xác định kiến trúc) : Syslog là một giao thức, nó là một phần của kiến trúc mạng hoàn chỉnh, với nhiều máy khách và máy chủ.

- Message format (định dạng tin nhắn) : syslog xác định cách định dạng tin nhắn. Điều này rõ ràng cần phải được chuẩn hóa vì các bản ghi thường được phân tích cú pháp và lưu trữ vào các công cụ lưu trữ khác nhau. Do đó, chúng ta cần xác định những gì một máy khách syslog có thể tạo ra và những gì một máy chủ nhật ký hệ thống có thể nhận được.

- Specifying reliability (chỉ định độ tin cậy) : syslog cần xác định cách xử lý các tin nhắn không thể gửi được. Là một phần của TCP/IP, syslog rõ ràng sẽ bị thay đổi trên giao thức mạng cơ bản (TCP hoặc UDP) để lựa chọn.

- Dealing with authentication or message authenticity (xử lý xác thực hoặc xác thực thư): syslog cần một cách đáng tin cậy để đảm bảo rằng máy khách và máy chủ đang nói chuyện một cách an toàn và tin nhắn nhận được không bị thay đổi.


## 3. Kiến trúc của Syslog

![Syslog architecture components](/Images/log-001.png)

Một máy Linux độc lập hoạt động như một máy chủ máy chủ syslog của riêng mình. Nó tạo ra dữ liệu nhật ký, nó được thu thập bởi rsyslog và được lưu trữ ngay vào hệ thống tệp.

- 1 syslog client/ 1 device: là 1 máy client sinh log

- 1 relay: nơi chuyển tiếp, forward log đến server

- 1 syslog server/ 1 collector: nơi tiếp nhận, lưu trữ log được gửi đến từ client

Đây là một tập hợp các ví dụ kiến trúc xung quanh nguyên tắc này:

![Architecture 1: one device, one collector](/Images/log-002.png)

![Architecture 2: multiple devices, one collector](/Images/log-003.png)

![Architecture 3: multiple devices, one collector, one relay](/Images/log-004.png)

## 4. Định dạng tin nhắn Syslog

![Syslog format explained](/Images/log-005.png)

Định dạng nhật ký hệ thống được chia thành ba phần, độ dài một thông báo không được vượt quá 1024 bytes:

- PRI: chi tiết các mức độ ưu tiên của tin nhắn (từ tin nhắn gỡ lỗi (debug) đến trường hợp khẩn cấp) cũng như các mức độ cơ sở (mail, auth, kernel).

- Header: bao gồm hai trường là TIMESTAMP và HOSTNAME, tên máy chủ là tên máy gửi nhật ký.

- MSG: phần này chứa thông tin thực tế về sự kiện đã xảy ra. Nó cũng được chia thành trường TAG và trường CONTENT.

  **4.1. Cấp độ cơ sở Syslog (Syslog facility levels)**
  
  - Một mức độ cơ sở được sử dụng để xác định chương trình hoặc một phần của hệ thống tạo ra các bản ghi.

  - Theo mặc định, một số phần trong hệ thống của bạn được cung cấp các mức facility như kernel sử dụng kern facility hoặc hệ thống mail của bạn bằng cách sử dụng mail facility.
      
  - Nếu một bên thứ ba muốn phát hành log, có thể đó sẽ là một tập hợp các cấp độ facility được bảo lưu từ 16 đến 23 được gọi là “local use” facility levels.
     
  - Ngoài ra, họ có thể sử dụng tiện ích của người dùng cấp độ người dùng (“user-level” facility), nghĩa là họ sẽ đưa ra các log liên quan đến người dùng đã ban hành các lệnh.

  Dưới đây là các cấp độ facility Syslog được mô tả trong bảng:

  ![Các cấp độ facility Syslog](/Images/log-006.png)
  
  **4.2. Mức độ cảnh báo của Syslog**
  
  - Mức độ cảnh báo của Syslog được sử dụng để mức độ nghiêm trọng của log event và chúng bao gồm từ gỡ lỗi (debug), thông báo thông tin (informational messages) đến mức khẩn cấp (emergency levels).

  - Tương tự như cấp độ cơ sở Syslog, mức độ cảnh báo được chia thành các loại số từ 0 đến 7, 0 là cấp độ khẩn cấp quan trọng nhất

  - Ngay cả khi các bản ghi được lưu trữ theo tên cơ sở theo mặc định, bạn hoàn toàn có thể quyết định lưu trữ chúng theo mức độ nghiêm trọng.

  - Nếu bạn đang sử dụng rsyslog làm máy chủ syslog mặc định, bạn có thể kiểm tra các thuộc tính rsyslog để định cấu hình cách các bản ghi được phân tách.

  Dưới đây là các mức độ nghiêm trọng của syslog được mô tả trong bảng:

  ![Các mức độ nghiêm trọng của syslog](/Images/log-007.png) 
  
  **4.3. PRI**
  
  Đoạn PRI là phần đầu tiên mà bạn sẽ đọc trên một tin nhắn được định dạng syslog.

  Phần PRI hay Priority là một số được đặt trong ngoặc nhọn, thể hiện cơ sở sinh ra log hoặc mức độ nghiêm trọng, là một số gồm 8 bit:

  - 3 bit đầu tiên thể hiện cho tính nghiêm trọng của thông báo.

  - 5 bit còn lại đại diện cho sơ sở sinh ra thông báo.

  ![PRI](/Images/log-008.png)
  
  **4.4. Header**

  ![Header](/Images/log-009.png)
 
  Header bao gồm:

  - TIMESTAMP: được định dạng trên định dạng của Mmm dd hh:mm:ss – Mmm, là ba chữ cái đầu tiên của tháng. Sau đó là thời gian mà thông báo được tạo ra giờ:phút:giây. Thời gian này được lấy từ thời gian hệ thống.

    -Chú ý: nếu như thời gian của server và thời gian của client khác nhau thì thông báo ghi trên log được gửi lên server là thời gian của máy client

  - HOSTNAME (đôi khi có thể được phân giải thành địa chỉ IP). Nó thường được đưa ra khi bạn nhập lệnh tên máy chủ. Nếu không tìm thấy, nó sẽ được gán cả IPv4 hoặc IPv6 của máy chủ.


## 5. Sysog gửi tin nhắn hoạt động như thế nào

> Chuyển tiếp nhật ký hệ thống là gì?

- Chuyển tiếp nhật ký hệ thống (syslog forwarding) bao gồm gửi log máy khách đến một máy chủ từ xa để chúng được tập trung hóa, giúp phân tích log dễ dàng hơn.

- Hầu hết thời gian, quản trị viên hệ thống không giám sát một máy duy nhất, nhưng họ phải giám sát hàng chục máy, tại chỗ và ngoài trang web.

- Kết quả là, việc gửi nhật ký đến một máy ở xa, được gọi là máy chủ ghi log tập trung, sử dụng các giao thức truyền thông khác nhau như UDP hoặc TCP.

> Syslog có sử dụng TCP hoặc UDP không?

- Syslog ban đầu sử dụng UDP, điều này là không đảm bảo cho việc truyền tin. Tuy nhiên sau đó IETF đã ban hành RFC 3195 (Đảm bảo tin cậy cho syslog) và RFC 6587 (Truyền tải thông báo syslog qua TCP). Điều này có nghĩa là ngoài UDP thì giờ đây syslog cũng đã sử dụng TCP để đảm bảo an toàn cho quá trình truyền tin.

- Syslog sử dụng port 514 cho UDP.

- Tuy nhiên, trên các triển khai log hệ thống gần đây như rsyslog hoặc syslog-ng, bạn có thể sử dụng TCP làm kênh liên lạc an toàn.

- Rsyslog sử dụng port 10514 cho TCP, đảm bảo rằng không có gói tin nào bị mất trên đường đi.

- Bạn có thể sử dụng giao thức TLS/SSL trên TCP để mã hóa các gói Syslog của bạn, đảm bảo rằng không có cuộc tấn công trung gian nào có thể được thực hiện để theo dõi log của bạn.
