Development Environment Setup
=============================


Get Centos 7.0
--------------------------------
Network source: http://centos.ustc.edu.cn/centos/7.0.1406/os/

Install Centos 6.4 in Virtualbox
--------------------------------
+ http://scm.zoomquiet.io/data/20130820213455/index.html
+ http://toolkt.com/site/virtualbox-shared-folders-with-centos-server-guest/
+ http://webees.me/installing-centos-6-4-in-virtualbox-and-setting-up-host-only-networking/


### Connect to a VirtualBox guest machine over the network
You must reconfigure the guest so it will get its own IP address:

+ Shutdown the guest
+ In the guest's VirtualBox settings, choose Network and switch the network adapter type to Bridged
+ Restart the guest
+ Read the guest's IP address from the output of ifconfig (Unix) or ipconfig (Windows)
+ You can now connect to this IP address using HTTP, SSH, etc.

Note that by doing this your VirtualBox guest is fully exposed on the local network, just like your host machine.

### Setup network for CentOS
+ Check MAC address in VM setting
+ Restart VM, check /etc/udev/rules.d/70-persistent-net.rules

##### Bridge (DHCP)
    vi /etc/sysconfig/network-scripts/ifcfg-eth0
    
    DEVICE="eth0"
    BOOTPROTO="dhcp"
    HWADDR="08:00:27:E9:33:84"
    NM_CONTROLLED="yes"
    ONBOOT="yes"
    TYPE="Ethernet"
    UUID="cfb31300-f7b8-4723-9c81-6ddbc1b976bf"

##### Host-only
    vi /etc/sysconfig/network-scripts/ifcfg-eth1
    
    DEVICE=eth1
    HWADDR=08:00:27:A1:D2:2E
    TYPE=Ethernet
    UUID=6ff9f9fa-6d66-4df7-beca-e279c4dc1863
    ONBOOT=yes
    NM_CONTROLLED=yes
    BOOTPROTO=none
    BROADCAST=192.168.56.255
    IPADDR=192.168.56.10
    NETMASK=255.255.255.0
    NETWORK=192.168.56.0
    IPV6INIT=no

+ Restart network service
    
    `service network restart`

### Install iptables in centos 7.0
http://stackoverflow.com/questions/24756240/how-can-i-use-iptables-on-centos-7

### CentOS 6: Open a port for iptables
+ Edit /etc/sysconfig/iptables
+ Add the following line above the first REJECT statement:
    `-A INPUT -m state --state NEW -p tcp --dport <port> -j ACCEPT`
+ Restart iptables
    `service iptables restart`
+ Test from remote machine
    `telnet <hostname> <port>`


Install softwares
-----------------
### Python 3.4
    yum install zlib-devel openssl-devel ncurses-devel sqlite-devel

    wget https://www.python.org/ftp/python/3.4.3/Python-3.4.3.tgz
    tar zxvf Python-3.4.3.tgz
    cd Python-3.4.3
    ./configure
    make; make install

### ssh
    yum search ssh
    yum install openssh-server.x86_64 openssh-clients.x86_64
    chkconfig sshd on
    service sshd start

### MongoDB 2.6
http://docs.mongodb.org/manual/tutorial/install-mongodb-on-red-hat-centos-or-fedora-linux/

+ Edit configuration: /etc/mongodb.conf
    + Enable http interface: httpinterface=true
    + Enable REST: rest=true
    + Change bind_ip: bind_ip=0.0.0.0
+ Enable ports (27017, 28017) in iptables for mongod (HTTP and REST)

### emacs (optional)
    yum install emacs (with X)
    yum install emacs-nox

Install Pycharm
---------------
http://www.tuicool.com/articles/MJnYJb


Flask Setup
-----------

#### iptables
    -A INPUT -p tcp -m tcp --dport 5000 -m state --state NEW,ESTABLISHED -j ACCEPT
    -A INPUT -p tcp -m tcp --dport 8000 -m state --state NEW,ESTABLISHED -j ACCEPT


Nginx Setup
-----------

#### Install
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7

#### Open port
http://www.binarytides.com/open-http-port-iptables-centos/

