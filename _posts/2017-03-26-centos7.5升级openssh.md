---
layout: post
title:  "centos7.5升级openssh"
categories: [实施工程]
tags: [centos openssh]
---

```c
需要可以连互联网


wget http://ftp.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-8.4p1.tar.gz

yum -y install xinetd
yum -y install telnet
yum -y install telnet-server

vi /etc/xinetd.d/telnet    修改 disable = yes 为 disable = no
如果不存在，就新建一个
# default: yes    
# description: The telnet server servestelnet sessions; it uses \   
# unencrypted username/password pairs for authentication.  
service telnet         
{    
	flags = REUSE    
	socket_type = stream    
	wait = no    
	user = root    
	server =/usr/sbin/in.telnetd    
	log_on_failure += USERID    
	disable = no   
}

systemctl restart xinetd.service
ps -ef | grep xinetd
ps -ef | grep telnet
systemctl stop firewalld

用telnet方式登录
gpowerx
Gpower@2019
su -

tar zxvf openssh-8.3p1.tar.gz
cd openssh-8.3p1


rpm -qa | grep pam

rpm -qa|grep zlib

rpm -qa |grep openssl


yum -y install gcc gcc++ && yum -y install pam-devel && yum -y install zlib-devel-1.2.7 && yum -y install openssl-devel


./configure --prefix=/usr --sysconfdir=/etc/ssh --with-md5-passwords --with-pam --with-zlib --with-openssl-includes=/usr --with-privsep-path=/var/lib/sshd 

for i in $(rpm -qa |grep openssh);do rpm -e $i --nodeps;done

rm -rf /etc/ssh

make && make install

cp contrib/redhat/sshd.init /etc/init.d/sshd

chkconfig --add sshd

chkconfig sshd on

chkconfig --list|grep sshd

sed -i "32a PermitRootLogin no" /etc/ssh/sshd_config






ssh -V

setenforce 0

vi /etc/selinux/config
SELINUX=disabled


systemctl restart sshd


使用ssh登录，成功后关闭telent相关

systemctl stop xinetd.service
ps -ef | grep xinetd
ps -ef | grep telnet
systemctl start firewalld
```
