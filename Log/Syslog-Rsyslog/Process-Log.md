# Quá trình phát triển

Syslog daemon: xuất bản năm 1980, syslog daemon có lẽ là triển khai đầu tiên từng được thực hiện và chỉ hỗ trợ một bộ tính năng giới hạn (chẳng hạn như truyền UDP). Nó thường được gọi là daemon sysklogd trên Linux.

Syslog-ng: xuất bản năm 1998, syslog-ng mở rộng tập hợp các khả năng của trình nền syslog gốc bao gồm chuyển tiếp TCP (do đó nâng cao độ tin cậy), mã hóa TLS và bộ lọc dựa trên nội dung. Bạn cũng có thể lưu trữ log vào cơ sở dữ liệu trên local để phân tích thêm.

Rsyslog – “The rocket-fast system for log processing” được bắt đầu phát triển từ năm 2004 bởi Rainer Gerhards rsyslog là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay rsyslog là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như: Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…
