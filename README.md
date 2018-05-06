# Các phần hướng dẫn trong nagios

## 1. Hướng dẫn cài đặt nagios

Hướng dẫn cài đặt Nagios [Tại đây](Document/Install_nagios_server_on_centos7.md")

## 2. Hướng dẫn cài đặt nagios_plugins

Hướng dẫn cài đặt Nagios_plugins [Tại đây](Document/Install_Nagios_Plugin.md)

## 3. Hướng dẫn monitor một hosts từ xa

Hướng dẫn giám sát một host từ xa [Tại đây](Document/Monitor_a_remote_host.md)

## ... Nhiều hosts với ví dụ mẫu: hosts group, contacts..., cảnh báo mail,

### Các thư mục quan trọng trong nagios

* Thư mục chứa các file cấu hình: /usr/local/nagios/etc/....

    + File cấu hình chính nagios.cfg 
    + File cấu hình khác cgi.cfg
    + File cấu hình định nghĩa các macro, $USERx$ cho các file khác lấy resource.cfg
    
* Thư mục chưa các file plugins: /usr/local/nagios/libexec/....

* Thư mục cấu hình các thực thể monitor: /usr/local/nagios/etc/objects/... 
 
    + Các file là host, device network được monitor như localhost.cfg, window.cfg, switch.cfg
    + File định nghĩa command để monitor hosts, servic như: command.cfg
    + File templates.cfg định nghĩa chung để cho các file khác kế thừa từ nó
    + File contacts.cfg định nghĩa các cách thức để gửi cảnh bảo mail, telegram.
    

    
