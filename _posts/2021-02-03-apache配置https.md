---
layout: post
title:  "apache配置https_篇二"
categories: [实施工程]
tags: [https]
---

```
1.自己生成证书
cd /usr/local/apache/conf
openssl genrsa 1024 > server.key

openssl req -new -key server.key > server.csr
会提示很多问题，看着输入就行

openssl req -x509 -days 365 -key server.key -in server.csr > server.crt
这是用步骤1,2的的密钥和证书请求生成证书server.crt，-days参数指明证书有效期，单位为天


将安装程序的modules下的loggers,ssl两个文件放到安装好的apache的modules下
cp -r /home/setup/httpd-2.4.29/modules/loggers/ /usr/local/apache/modules/
cp -r /home/setup/httpd-2.4.29/modules/ssl/ /usr/local/apache/modules/

注意openssl，gcc，gcc-c++
 yum install openssl-devel
cd到服务端的modules/ssl目录;执行命令： apxs -i -c -a -D HAVE_OPENSSL=1 -I openssl -lcrypto -lssl -ldl *.c 即可
cd /usr/local/apache/modules/ssl/
/usr/local/apache/bin/apxs -a -i -c -L /usr/lib/openssl/engines/lib -c *.c -lcrypto -lssl -ldl
/usr/local/apache/bin/apxs  -a -i -DHAVE_OPENSSL=1 -I /usr/include/openssl -L /usr/lib64/openssl -c *.c -lcrypto -lssl -ldl



2.修改配置文件
httpd.conf
找到 LoadModule socache_shmcb_module modules/mod_socache_shmcb.so，把前面的注释去掉
找到 LoadModule ssl_module modules/mod_ssl.so ，把前面的注释去掉
找到 LoadModule rewrite_module modules/mod_rewrite.so，把前面的注释去掉

找到 Include conf/extra/httpd-ssl.conf，把前面的注释去掉

修改extra/httpd-ssl.conf
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
