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
        check_command                   check_ping!100.0,20%!500.0,60%
        }
        
        define service {
        use                             generic-service
        host_name                       yourhost
        service_description             SSH
        check_command                   check_ssh
        notifications_enabled           0
        }
     
* B5: Restart serivce nagios
       
       systemctl restart nagios
       
#### Trên remote hosts

Cài nrpe trên hosts:

      sudo yum install epel-release
      sudo yum install nrpe nagios-plugins-all
      allowed_hosts=127.0.0.1,IP_server_nagios
      systemctl start nrpe.service
      systemctl enable nrpe.service
      
      


