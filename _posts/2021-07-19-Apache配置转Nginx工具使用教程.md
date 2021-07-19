---

layout: post
title:  "Apache配置转Nginx工具使用教程"
categories: [实施工程]
tags: [Apache,Nginx]

---

### 工具下载：<https://github.com/leeleander/apache2nginx>

github上也有相关教程可参考；

本地151服务器（192.168.2.151）上安装了该工具，安装方法参考上面官方教程或百度下。  

### 使用方法：  

将需要被转为Nginx的Apache的http.conf文件上传到151上；

进入`/home/tools/apache2nginx`目录下 键入命令： `./apache2nginx -f /home/tools/httpd.conf`  

-f后面路径为你上传的http.conf位置  

转换成功会有Completed XXX 提示，并在该工具安装目录下`/home/tools/apache2nginx`生成一个名为 `nginx.conf`的文件，该文件即为转换后的文件。