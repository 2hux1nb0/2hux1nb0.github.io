---

layout: post
title:  "Linux彻底删除MySQL"
categories: [Linux]
tags: [Linux]

---
首先在终端中查看MySQL的依赖项：`dpkg --list|grep mysql`

卸载： `sudo apt-get remove mysql-common`

卸载：`sudo apt-get autoremove --purge mysql-server-5.7`

清除残留数据：`dpkg -l|grep ^rc|awk '{print$2}'|sudo xargs dpkg -P`

再次查看MySQL的剩余依赖项：`dpkg --list|grep mysql`

继续删除剩余依赖项，如：`sudo apt-get autoremove --purge mysql-apt-config`
至此已经没有了MySQL的依赖项，彻底删除，Good Luck

详细教程可参考：<https://blog.csdn.net/fanrongwoaini/article/details/107518693?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.nonecase>