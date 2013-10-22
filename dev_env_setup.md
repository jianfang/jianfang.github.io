Development Environment Setup
=============================

Install Centos 6.4 in Virtualbox
--------------------------------
http://webees.me/linux/set-up-develop-environment-with-centos-6-4-in-virtualbox/

Install ssh
-----------
    yum search ssh
    yum install openssh-server.x86_64 openssh-clients.x86_64
    chkconfig sshd on
    service sshd start

Install emacs
-------------
    yum install emacs (with X)
    yum install emacs-nox

