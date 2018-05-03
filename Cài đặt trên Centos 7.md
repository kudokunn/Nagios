B1: Disable selinux
B2: Cài các gói cần thiết: 
    yum install -y gcc glibc glibc-common wget unzip httpd php gd gd-devel perl
 B3: Download nagios: 
    
    cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.3.4.tar.gz
tar xzf nagioscore.tar.gz
