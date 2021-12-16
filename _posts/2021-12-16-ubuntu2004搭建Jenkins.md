---  

layout: post 
title:  "ubuntu20.04搭建Jenkins教程" 
categories: [Jenkins] 
tags: [ubuntu]  

---

### ubuntu20.04安装Jenkins  

1. 将存储库密钥添加到系统：`wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -`  

2. `sudo sh -c ‘echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list’`  

上面两条命令执行后并无什么反馈，没事，是正常的  

3. `sudo apt-get update`  执行update后如果提示GPG错误，参考该文档解决<https://2hux1nb0.github.io/2021/12/16/ubuntu%E5%AE%89%E8%A3%85Jenkins%E6%8A%A5GPG%E9%94%99%E8%AF%AF%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95>

4. `sudo apt-get install jenkins`  

5. 配置Jenkins端口：修改`/etc/default/jenkins`,  配置默认端口8080改为8082（或者其他不冲突的端口号），**注：这里jenkins默认只读的，端口默认8080会和tomcat或Apache冲突，需要用root身份进行修改 `sudo vim /etc/default/jenkins`**

   改为：`HTTP_PORT=8082`  

   还有一个文件也要改8082，在/etc/init.d/jenkins中，一共这两个地方，改完保存退出，重启Jenkins。

   Jenkins启动/重启/停止命令：  

   查看状态：`systemctl status jenkins.service`

   启动 `service jenkins start`

   重启 `service jenkins restart`

   停止 `service jenkins stop`  

   查看Jenkins进程：`ps -ef |grep jenkins`  

   访问：`http://localhost:8082` 即可访问Jenkins界面，初次访问需按照页面的引导去操作，注册用户，让你输入一个密码，这个密码在`/var/log/jenkins`目录下的jenkins.log文件中，键入命令`gedit jenkins.log` 找到下图红框处，为密码，粘贴上去即可。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ce2dc2eec6fd47b2a943716530b6a119.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6buR5pyx6ZuA,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


至此，Jenkins搭建完成。