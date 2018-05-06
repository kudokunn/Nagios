### Muốn monitor remote host cần có addons NRPE trên cả Server và remote host.

#### Addons là gì:

Là các gói packages có chức năng để mở rộng chức năng của Nagios-Server như
 + Quản lý các file cấu hình thông qua giao diện Web
 + Giám sát các remote host
 + Đệ trình passive check từ remote hosts

### Addon NRPE:

+ Là addon cho phép thực thi các plugin trên remote Linux host từ xa. Cho phép monitor các thuộc tính, tài nguyên trên các remote Linux từ xa.

+ Cần phải cài addon này trên cả Server và Remote hosts

### Cài đặt 

#### Trên server: 

* B1: Tải từ source và cài đặt
    
      cd ~
      curl -L -O http://downloads.sourceforge.net/project/nagios/nrpe-2.x/nrpe-2.15/nrpe-2.15.tar.gz
      tar xvf nrpe-*.tar.gz
      cd nrpe-*
      ./configure --enable-command-args --with-nagios-user=nagios --with-nagios-group=nagios --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu

      make all
      sudo make install
      sudo make install-xinetd
      sudo make install-daemon-config
 
* B2: Chỉnh file cấu hình: 

      sudo vi /etc/xinetd.d/nrpe
      only_from = 127.0.0.1 IP_nagios_server
      
* B3: Cấu hình command NRPE

      sudo vi /usr/local/nagios/etc/objects/commands.cfg
      define command{
        command_name check_nrpe
        command_line $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
       }
     
 * B4: Định nghĩa file cấu hình cho remote hosts
 
       vi /usr/local/nagios/etc/servers/remotehost.cfg
       ### thêm vào file đó
       define host {
        use                             linux-server
        host_name                       remotehost
        alias                           My first Apache server
        address                         IP_remote_host
        max_check_attempts              5
        check_period                    24x7
        notification_interval           30
        notification_period             24x7
       }
       # Định nghĩa cả service trên đó:
       define service {
        use                             generic-service
        host_name                       yourhost
        service_description             PING
        check_command                   check_ping!100.0,20%!500.0,60% ### sử dụng ngay plugin tại server kết nối đến host từ xa để kiểm tra
        }
        
        define service {
        use                             generic-service
        host_name                       yourhost
        service_description             SSH
        check_command                   check_ssh ### sử dụng ngay plugin tại server kết nối đến host từ xa để kiểm tra
        notifications_enabled           0
        }
        # => chưa phải là thực chất của nrpe, nó phải là sử dụng lệnh "check_nrpe![tên command được định nghĩa ở client trong file nrpe.conf]"    
  
        define service {
        use                             generic-service
        host_name                       LoadBalancing
        service_description             Root partition
        check_command                   check_local_disk!20%!10%!/ # cái này là plugins check_disk luôn check tại local (ý nghĩa cái local là vì tùy chọn này của nó là không có tham số -h (hosts) => kết quả giống với localhost dù có được định nghĩa nó tại LoadBalancing. Nó giống như check_local_user nó chỉ check tại nó, nếu remote host thì cần check_nrpe!check_user<tại remote host và được định nghĩa trong file cấu hình nrpe.conf tại remote hosts>
         
        notifications_enabled           0 # tắt cảnh báo cho nó
        }
        
       ##################### bắt đầu là check_nrpe cho host từ xa
       
       define service {
               use                             generic-service
               host_name                       LoadBalancing
               service_description             Users
               check_command                   check_nrpe!check_users
       #        notifications_enabled           0
       }

       define service {
               use                             generic-service
               host_name                       LoadBalancing
               service_description             Total_process
               check_command                   check_nrpe!check_total_procs
       #        notifications_enabled           0
       }
       
       define service {
        use                             generic-service
        host_name                       LoadBalancing
        service_description             Load
        check_command                   check_nrpe!check_load
        #        notifications_enabled           0
        }

        define service {
                use                             generic-service
                host_name                       LoadBalancing
                service_description             I/O
                check_command                   check_nrpe!check_io
        #        notifications_enabled           0
        }

        define service {
                use                             generic-service
                host_name                       LoadBalancing
                service_description             Network
                check_command                   check_nrpe!check_network
        #        notifications_enabled           0
        }

        define service {
        use                             generic-service
        host_name                       LoadBalancing
        service_description             Uptime
        check_command                   check_nrpe!check_uptime
        #        notifications_enabled           0
        }

        
* B5: Restart serivce nagios
       
       systemctl restart nagios
       
#### Trên remote hosts

* B1: Cài nrpe trên hosts:

      sudo yum install epel-release
      sudo yum install nrpe nagios-plugins-all
  
* B2: Cấu hình: vim /etc/nagios/nrpe.conf
      
       allowed_hosts=127.0.0.1,IP_server_nagios
      
* B3: Định nghĩa các command tại nrpe.conf

      # Check disk
      command[check_disk]=/usr/lib64/nagios/plugins/check_linux_stats.pl -D -w 10 -c 5 -u % -p /,/data,/boot
      # Check load
      command[check_load]=/usr/lib64/nagios/plugins/check_linux_stats.pl -L -w 10,8,5 -c 20,18,15
      # Check IO disk
      command[check_io]=/usr/lib64/nagios/plugins/check_linux_stats.pl -I -w 100,70 -c 150,100 -p sda3,sda1,sda2
      # Check network IO
      command[check_network]=/usr/lib64/nagios/plugins/check_linux_stats.pl -N -w 30000000 -c 45000000 -p eth0
      # Check uptime
      command[check_uptime]=/usr/lib64/nagios/plugins/check_linux_stats.pl -U -w 5
      # Check process
      command[check_procs]=/usr/lib64/nagios/plugins/check_procs -w 800 -c 1000

* B4: Khởi động lại serivce

      systemctl start nrpe.service
      systemctl enable nrpe.service
      
### Check tại: http://IP_server_nagios/nagios

![](/image/1.PNG)
      
      


