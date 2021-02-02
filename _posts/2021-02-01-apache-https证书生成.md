---
layout: post
title:  "apache https证书生成"
categories: [实施工程]
---

 自己生成证书  

申请Let's Encrypt永久免费SSL证书  

  Let's Encrypt简介  
Let's Encrypt作为一个公共且免费SSL的项目逐渐被广大用户传播和使用，是由Mozilla、Cisco、Akamai、IdenTrust、EFF等组织人员发起，主要的目的也是为了推进网站从HTTP向HTTPS过度的进程，目前已经有越来越多的商家加入和赞助支持。  
Let's Encrypt免费SSL证书的出现，也会对传统提供付费SSL证书服务的商家有不小的打击。到目前为止，Let's Encrypt获得IdenTrust交叉签名，这就是说可以应用且支持包括FireFox、Chrome在内的主流浏览器的兼容和支持，虽然目前是公测阶段，但是也有不少的用户在自有网站项目中正式使用起来。  

步骤如下：  

 第一、安装Let's Encrypt前的准备工作  
```
检查系统是否安装git,如果已经自带有git会出现git版本号，没有则需要我们自己安装  
git  --version   
git 安装  
yum install git  
检查Python的版本是否在2.7以上  
python -v //2.6版本  
安装python所需的包  
yum install zlib-devel  
yum install bzip2-devel  
yum install openssl-devel  
yum install ncurses-devel  
yum install sqlite-devel  
获取到Python  
cd /usr/local/src  
wget https://www.python.org/ftp/python/2.7.12/Python-2.7.12.tar.xz  
解压Python2.7.12  
tar -zxvf Python-2.7.12.tar.xz  
编译python  
cd Python-2.7.12/  
./configure --prefix=/usr/local/python2.7  
make && make install  
建立链接  
ln -s /usr/local/python2.7/bin/python2.7 /usr/local/bin/python  
解决系统 Python 软链接指向 Python2.7 版本后，因为yum是不兼容  Python 2.7的，所需要指定 yum 的Python版本  
vi /usr/bin/yum   
将头部的  
!/usr/bin/python
改成
!/usr/bin/python2.6.6
```
 第二、获取Let's Encrypt免费SSL证书
```
获取letsencrypt  
git clone https://github.com/letsencrypt/letsencrypt  
进入letsencrypt目录  
cd letsencrypt  
生成证书  
./certbot-master/letsencrypt-auto certonly --standalone --email xxx@xxx.com -d www.nudt.edu.cn -d english.nudt.edu.cn  

0 0 1 */2 * /data/ma/certbot-master/letsencrypt-auto certonly --standalone --email xxx@xxx.com -d www.nudt.edu.cn -d english.nudt.edu.cn
```

 第三、Let's Encrypt免费SSL证书获取与应用
在完成Let's Encrypt证书的生成之后，我们会在"`/etc/letsencrypt/live/xxx.me/`"域名目录下有4个文件就是生成的密钥证书文件。  
`cert.pem  - Apache服务器端证书`  
`chain.pem  - Apache根证书和中继证书`  
`fullchain.pem  - Nginx所需要ssl_certificate文件`  
`privkey.pem - 安全证书KEY文件`  
如果我们使用的Nginx环境，那就需要用到`fullchain.pem`和`privkey.pem`两个证书文件，在部署Nginx的时候需要用到。  
在Nginx环境中，只要将对应的`ssl_certificate和ssl_certificate_key`路径设置成我们生成的2个文件就可以。  

打开linux配置文件，找到`HTTPS 443`端口配置的`server`  
```
 ssl_certificate /etc/letsencrypt/live/zhaoheqiang.me/fullchain.pem;  
 ssl_certificate_key /etc/letsencrypt/live/zhaoheqiang.me/privkey.pem;  
```


 第四、解决Let's Encrypt免费SSL证书有效期问题
Let's Encrypt证书是有效期90天的，需要我们自己手工更新续期才可以。  

命令如下：  
```
 ./letsencrypt-auto certonly --renew-by-default --email quiniton@163.com -d xxx.me -d www.xxx.me
```
这样我们在90天内再去执行一次就可以解决续期问题，这样又可以继续使用90天。如果我们怕忘记的话也可以利用linux  crontab定时执行更新任务

2.修改配置文件
`httpd.conf ` 
找到 `LoadModule socache_shmcb_module modules/mod_socache_shmcb.so`，把前面的注释去掉  
找到 `LoadModule ssl_module modules/mod_ssl.so` ，把前面的注释去掉  
找到 `LoadModule rewrite_module modules/mod_rewrite.so`，把前面的注释去掉  

找到 `Include conf/extra/httpd-ssl.conf`，把前面的注释去掉  

修改`extra/httpd-ssl.conf`  
```
<VirtualHost _default_:443>
这段开始，就是虚拟主机ssl的配置
SSLCertificateFile "/usr/local/apache/conf/server.crt"
SSLCertificateKeyFile "/usr/local/apache/conf/server.key"
这是https相关证书的配置路径

在最后添加
RewriteEngine on
RewriteCond %{SERVER_PORT} !^443$
RewriteRule ^/?(.*)$ https://%{SERVER_NAME}/$1 [L,R]
当输入http时，自动跳到https
```