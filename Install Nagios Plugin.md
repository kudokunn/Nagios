### 1: Plugin là gì:

* Không giống như mấy tool monitor khác, Nagios để monitor các serivce hay host từ xa cần một chương trình ngoài (external programs) được gọi là Plugins để thực hiện các công việc giám sát

* Plugins là các script có thể được thực thi (Python, PHP, Shell, ...), nó có thể chạy từ command line để kiểm tra trạng thái của host hoặc service, và Nagios sẽ sử dụng kết quả của plugins trả về để xác định trạng thái của host và serivces và nagios sẽ tạo các các action như mong muốn: send notification, handle events ....

### Cài đặt: 

* B1: Install các gói cần thiết: 
  
      yum install -y gcc glibc glibc-common make gettext automake autoconf wget openssl-devel net-snmp net-snmp-utils epel-release
      yum install -y perl-Net-SNMP
      
 * B2: Download plugins:
 
        cd /tmp
        wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.2.1.tar.gz
        tar zxf nagios-plugins.tar.gz
        
 * B3: Cài đặt:
 
        cd /tmp/nagios-plugins-release-2.2.1/
        ./tools/setup
        ./configure
        make
        make install
        
  * B4: Restart lại serive và enable serivce
  
        systemctl nagios restart
        systemctl nagios enable
        
 ### Check lại kết quả tại trang: http://IP_server_nagios/nagios
