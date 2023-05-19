# Bypass User Account Control trên Windows 10

UAC (User Account Control) là một tính năng bảo mật windows buộc bất kỳ tiến trình mới nào chạy ở chế độ không nâng cao theo mặc định. Bất kỳ quá trình nào được thực hiện bởi bất kỳ người dùng nào bao gồm cả quản trị viên đều phải tuân theo các quy tắc của UAC, tức là 'Không tin tưởng bất kỳ người dùng nào đang chạy quy trình'. Nếu các hành động phải được thực hiện, thì nó phải được ủy quyền.

Nếu người dùng muốn chạy tiến trình ở chế độ đặc quyền nâng cao (nói cách khác là với đặc quyền cấp quản trị), thì UAC sẽ tự hiển thị với một hộp thoại để xác nhận xem tiến trình có được phép chạy hay không. Hầu hết các bạn có thể đã thấy hộp thoại này bất cứ khi nào thực hiện bất kỳ ứng dụng nào có đặc quyền cấp quản trị.

> Tại sao phải Enable UAC?
> 
> Lấy kịch bản trong đó người dùng tải xuống tệp ứng dụng độc hại và người dùng nằm trong nhóm quản trị viên. Nếu UAC không được bật thì ứng dụng có thể dễ dàng chạy với các đặc quyền quản trị mà không cần ủy quyền.
 
Trong trường hợp này, chúng ta sẽ xem xét **fodhelper.exe**, đây là tệp thực thi mặc định của Windows được yêu cầu để quản lý các tính năng tùy chọn của Windows, ngôn ngữ bổ sung, v.v. Nó cũng là một chương trình thực thi tự động. Điều này có nghĩa là quản trị viên sẽ không được UAC nhắc thực hiện các tác vụ nâng cao.

- Chúng ta tạo các Rule để cảnh báo cho chúng ta biết Bypass UAC có thành công hay không. Chúng ta sẽ thêm Rule vào file local_rules.xml trên Wazuh manager.

- Để bắt đầu, hãy tạo một thư mục tạm thời để test. Và chúng ta cần làm là có được một vỏ đặc quyền thấp trên mục tiêu. Đối với mục đích trình diễn, ta sẽ tạo một payload đơn giản bằng cách sử dụng MSFvenom và lưu nó dưới dạng tệp thực thi để chạy trên mục tiêu.

![tạo một thư mục tạm thời](/Images/BypassUAC-001.png)

- Đây là những gì đang xảy ra trong lệnh trên:
  - -P chỉ định tải trọng
  - lhost là máy cục bộ của chúng tôi để kết nối lại
  - lport là cổng địa phương để kết nối với
  - -f đặt định dạng
  - -o chỉ định tệp đầu ra

- Bây giờ tệp đã được lưu, ta cần thiết lập một trình nghe để nó kết nối lại sau khi nó được thực thi. 

- Mở terminal và kích hoạt Metasploit bằng lệnh msfconsole.

- Đặt payload, lhost và lport như sau và nhập run để bắt đầu lắng nghe các kết nối đến.

![Đặt payload, lhost và lport](/Images/BypassUAC-005.png)

- Bây giờ, có thể bắt đầu một máy chủ HTTP để lưu trữ tệp lúc trước, vì vậy tất cả những gì nạn nhân phải làm là kết nối với chúng ta, tải xuống tệp và chạy nó.

- Chúng ta có thể khởi động Python vì nó có một mô-đun tích hợp được gọi là SimpleHTTPServer, dễ sử dụng và có thể chạy từ mọi nơi mà không cần bất kỳ thiết lập nào. Bắt đầu nó bằng lệnh sau.

![Khởi chạy SimpleHTTPServer](/Images/BypassUAC-004.png)

- Bây giờ tất cả những gì nạn nhân phải làm là kết nối với máy của chúng ta trên cổng 8000 để lấy tệp. Trên mục tiêu, duyệt đến địa chỉ IP của máy tấn công và tải xuống tệp.

![Khởi chạy SimpleHTTPServer](/Images/BypassUAC-002.png)

-	Sau đó, chỉ cần lưu nó và chạy nó:

![Save and Run](/Images/BypassUAC-003.png)

- Nếu mọi thứ diễn ra suôn sẻ, chúng ta sẽ thấy một phiên Meterpreter được thiết lập trở lại trên trình xử lý của chúng ta.

![Attack](/Images/BypassUAC-006.png)

-	Tại thời điểm này, chúng ta có thể dừng máy chủ Python vì chúng ta đã kết nối thành công với mục tiêu.

**Cố gắng leo thang đặc quyền**

- Bây giờ chúng ta có một phiên Meterpreter, chúng ta có thể thấy chúng ta đang chạy người dùng nào với lệnh **getuid**. Và thử leo thang bằng cách sử dụng lệnh **getsystem**.
 
![Fail](/Images/BypassUAC-007.png)
 
- **Và nó thất bại**. Chúng ta có thể thử bằng phương pháp khác.

**Bỏ qua UAC bằng Fodhelper**

- Chúng ta có thể sử dụng mô-đun Metasploit để bỏ qua tính năng UAC trên Windows. Nhập background để làm như vậy.

- Chúng ta khai thác **bypassuac_fodhelperc** - tải mô-đun bằng lệnh use.

- Đặt payload, lhost và lport và set session như sau
 
![Đặt payload, lhost và lport và set session](/Images/BypassUAC-008.png)
 
- Hãy xem các tùy chọn để xem những gì chúng ta cần. 
 
![options](/Images/BypassUAC-021.png)
 
- Có vẻ như chúng ta đang sử dụng Windows 64 bit. Nên đặt lại target.
 
 ![set target](/Images/BypassUAC-022.png)
  
- Hãy gõ chạy để khởi chạy khai thác.
 
![exploit](/Images/BypassUAC-023.png)
  
- Chúng ta có thể thấy nó kiểm tra cấp UAC và nếu người dùng là một phần của nhóm Quản trị viên và một phiên mới được mở thành công. Hãy chạy **getuid** một lần nữa.
 
![sysinfo and getuid](/Images/BypassUAC-024.png)
   
- Chúng ta có thể thấy chúng ta vẫn là **duc** — việc khai thác không tự động thả chúng ta vào tài khoản Hệ thống. Nhưng bây giờ nếu chúng ta chạy **getsystem**, chúng ta có thể vượt qua UAC thành công và leo thang các đặc quyền.

- Và bây giờ chúng tôi có thể xác nhận rằng cuối cùng chúng tôi cũng có quyền truy cập Hệ thống.

 ![getsystem](/Images/BypassUAC-025.png)
  
- Trên máy nạn nhân, có command prompt quyền quản trị hệ thống đang mở, mặc dù máy nạn nhân không chạy nó.
 
 ![cmd](/Images/BypassUAC-011.png)
   
- Ta sẽ nhận được cảnh báo khi ai đó muốn Bypass UAC.
 
  ![cảnh báo](/Images/BypassUAC-012.png)


- **Tiện ích nâng cao tự động đã biết FodHelper.EXE có thể đã được sử dụng để bỏ qua UAC.**
 
![thông tin chi tiết](/Images/BypassUAC-013.png)

![thông tin chi tiết](/Images/BypassUAC-014.png)

![thông tin chi tiết](/Images/BypassUAC-015.png)

![thông tin chi tiết](/Images/BypassUAC-016.png)
     

- **fodhelper.exe được sử dụng để bỏ qua UAC và thực thi phần mềm độc hại.**
 
![thông tin chi tiết](/Images/BypassUAC-017.png)

![thông tin chi tiết](/Images/BypassUAC-018.png)

![thông tin chi tiết](/Images/BypassUAC-019.png)

![thông tin chi tiết](/Images/BypassUAC-020.png)
 





