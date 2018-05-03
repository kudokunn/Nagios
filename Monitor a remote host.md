### Muốn monitor remote host cần có addons NRPE trên cả Server và remote host.

#### Addons là gì:

Là các gói packages có chức năng để mở rộng chức năng của Nagios-Server như
 + Quản lý các file cấu hình thông qua giao diện Web
 + Giám sát các remote host
 + Đệ trình passive check từ remote hosts

### Addon NRPE:

+ Là addon cho phép thực thi các plugin trên remote Linux host từ xa. Cho phép monitor các thuộc tính, tài nguyên trên các remote Linux từ xa.

+ Cần phải cài addon này trên cả Server và Remote hosts

### Cài đặt trên server: 

* B1: Tải từ suorce
    
    cd ~
    curl -L -O http://downloads.sourceforge.net/project/nagios/nrpe-2.x/nrpe-2.15/nrpe-2.15.tar.gz
