---  

layout: post 
title:  "ubuntu安装Jenkins报GPG秘钥错误解决办法" 
categories: [BugKiller] 
tags: [ubuntu]  

---
> W: GPG 错误:https://pkg.jenkins.io/debian-stable binary/ Release: 由于没有公... 

`sudo apt-get update`时，会遇到上面提示的错误，解决方法;

`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 23E7166788B63E1E` 此处末尾的一串数字是示例，不要全部复制，`23E7166788B63E1E`要被替换为上面错误提示里的一串字符。
