* B1: Disable selinux
* B2: Cài các gói cần thiết: 

         yum install -y gcc glibc glibc-common wget unzip httpd php gd gd-devel perl
         
* B3: Download nagios: 
    
       cd /tmp
       wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.3.4.tar.gz
       tar xzf nagioscore.tar.gz
       
* B4: Biên dịch hay cài đặt từ mã nguồn:
    
        cd /tmp/nagioscore-nagios-4.3.4/
        ./configure # mục đích để check các điều kiện trong server có đủ để cài phần mềm này
        make all

* B5: Tạo user và group
        
        useradd nagios
        usermod -a -G nagios apache

* B6: Cài đặt nhị phân: 
        
        make install # để cài đặt các nhị phân tại usr/local/nagios/sbin_or_bin/...
        
* B7: Cài đặt service: 
         
        make install-init 
        systemctl enable nagios.service
        systemctl enable httpd.service

* B8: Cài đặt chế độ command: 

        make install-commandmode
        
* B9: Cài đặt file cấu hình:

        make install-config
        
* B10: Cấu hình file configures apache: nagios.conf trong thư mục httpd

        make install-webconf
        
* B11: Mở port filewall 80 để có thể reach đến web GUI nagios của server từ xa
    
        firewall-cmd --zone=public --add-port=80/tcp
        firewall-cmd --zone=public --add-port=80/tcp --permanent
        firewall-cmd --reload
        
* B12: Tạo basic xác thực để vào nagios server: vì nagios không giống như zabbix, nó không đòi đăng nhập lúc vào

        htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
        ### không có file này sẽ lỗi: Internal Server Error
        
* B13: Start các service:

        systemctl httpd start
        systemctl nagios start
        
* B14 Test: http://IP_server_installed_nagios/nagios. But a error appear: 

        (No output on stdout) stderr: execvp(/usr/local/nagios/libexec/check_load, ...) failed. errno is 2: No such file or directory 

=> Chưa có plugin để monitor chính con server nagios này, cần cài Nagios Plugins cho nó

        


