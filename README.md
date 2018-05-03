# Các phần hướng dẫn trong nagios

### Các thư mục quan trọng trong nagios

* Thư mục chứa các file cấu hình: /usr/local/nagios/etc/....

    + File cấu hình chính nagios.cfg 
    + File cấu hình khác cgi.cfg
    + File cấu hình định nghĩa các macro, $USERx$ cho các file khác lấy
    
* Thư mục chưa các file plugins: /usr/local/nagios/libexec/....

* Thư mục cấu hình các thực thể monitor: /usr/local/nagios/etc/objects/... 
 
    + Các file là host, device network được monitor như localhost.cfg, window.cfg, switch.cfg
    + File định nghĩa command để monitor hosts, servic như: command.cfg
    + File templates.cfg định nghĩa chung để cho các file khác kế thừa từ nó
    + File contacts.cfg định nghĩa các cách thức để gửi cảnh bảo mail, telegram.
    
