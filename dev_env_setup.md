Development Environment Setup
=============================

Install Centos 6.4 in Virtualbox
--------------------------------
http://webees.me/linux/set-up-develop-environment-with-centos-6-4-in-virtualbox/

### Connect to a VirtualBox guest machine over the network
You must reconfigure the guest so it will get its own IP address:

+ Shutdown the guest
+ In the guest's VirtualBox settings, choose Network and switch the network adapter type to Bridged
+ Restart the guest
+ Read the guest's IP address from the output of ifconfig (Unix) or ipconfig (Windows)
+ You can now connect to this IP address using HTTP, SSH, etc.

Note that by doing this your VirtualBox guest is fully exposed on the local network, just like your host machine.

### CentOS 6: Open a port for iptables
+ Edit /etc/sysconfig/iptables
+ Add the following line above the first REJECT statement:
    `-A INPUT -m state --state NEW -m tcp -p tcp --dport <port> -j ACCEPT`
+ Restart iptables

    `service iptables restart`
+ Test from remote machine

    `telnet <hostname> <port>`

Install softwares
-----------------
### ssh
    yum search ssh
    yum install openssh-server.x86_64 openssh-clients.x86_64
    chkconfig sshd on
    service sshd start

### emacs
    yum install emacs (with X)
    yum install emacs-nox

### MongoDB
http://docs.mongodb.org/manual/tutorial/install-mongodb-on-red-hat-centos-or-fedora-linux/

+ Enable ports (27017, 28017) in iptables for mongod, HTTP and REST
+ Enable REST by adding the following in configuration file (/etc/mongodb.conf) 
    `rest = true`

