# Phát hiện malware với Wazuh tích hợp VirusTotal

Sử dụng Wazuh tích hợp VirusToTal

- Wazuh tự động gửi yêu cầu tới API VirusTotal với các hàm băm của tệp được tạo hoặc sửa đổi trong các thư mục được giám sát. Điều này được thực hiện theo các bước sau:

  - Giám sát tệp: Mô-đun FIM phát hiện việc sửa đổi, xóa hoặc bổ sung tệp và kích hoạt cảnh báo.

  - Yêu cầu API VirusTotal: Sau khi FIM kích hoạt cảnh báo, trình quản lý Wazuh sẽ truy vấn API VirusTotal với hàm băm của tệp.

  - Cảnh báo: Nếu có sự trùng khớp từ VirusTotal, trình quản lý Wazuh sẽ tạo một cảnh báo.

- Thực hiện giám sát thư mục (File integrity monitoring):

  - Ta thêm tệp cấu hình sau vào thư mục C:\Program Files (x86)\ossec-agen\ossec.conf trên Windows agent và group configuration trên Wazuh manager:

![Cấu hình ossec.conf](/Images/wazuh-007.png)

![Cấu hình ossec.conf](/Images/wazuh-009.png)

![Cấu hình wazuh manager](/Images/wazuh-008.png)

  - Mỗi khi thư mục được giám sát có sự thay đổi như thêm mới, xóa, sửa đổi file, nó sẽ hiển thị thông báo trên Wazuh

- Thêm

![Thêm file](/Images/wazuh-010.png)

- Xóa
 
 ![Xóa file](/Images/wazuh-011.png)
 
- Để quét các tệp này bằng VirusTotal, ta cần thêm tệp cấu hình API bên dưới vào /var/ossec/etc/ossec.conf tệp cấu hình Wazuh manager.
 
 ![Thêm tệp cấu hình API](/Images/wazuh-012.png)

- Khởi động lại dịch vụ Wazuh manager sau khi thêm cấu hình VirusTotal. Thực hiện lệnh: systemctl restart wazuh-manager

- Sau khi tích hợp VirusTotal, ta sẽ nhận được cảnh báo bất cứ khi nào phần mềm độc hại được thêm vào thư mục được giám sát.
 
 ![Virustotal đã quét được file độc hại được thêm vào thư mục giám sát](/Images/wazuh-013.png)
 
 ![Chi tiết log của VirusTotal khi quét malware](/Images/wazuh-014.png)

 

