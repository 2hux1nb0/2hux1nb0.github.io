---
layout: post
title:  "linux rsync"
categories: [Linux]
---

rsync的备份指南： 
需求：用rsync完成异地文件的同步 
难点：如何建立异地信任关系 

1、在A主机上（rsync服务器）上编译安装rsync，需要版本在2.4.3以上（http://rsync.samba.org），在/etc目录下建立rsyncd.conf文件，内容如下： 
uid = nobody  
gid = nobody  
use chroot = no  # 不使用chroot  
max connections = 4  # 最大连接数为4 
log　file　=　/var/log/rsyncd.log  
pid　file　=　/var/run/rsyncd.pid  
lock　file　=　/var/run/rsync.lock  # 日志记录文件 

[test]  # 这里是认证的模块名，在客户端需要指定 
　　　path　=　/home/test  # 需要同步的目录 
　　　comment　=　test　folder  
　　　uid　=　root  
　　　ignore　errors  # 可以忽略一些无关的IO错误 
　　　read　only　=　yes  # 只读 
　　　list　=　no  # 不允许列文件 
　　　auth　users　=　rsynctest # 认证的用户名，如果没有这行，则表明是匿名 
　　　secrets　file　=　/etc/test.scrt  # 认证用户密码文件 

2、在/etc下建立test.scrt文件，输入： 
用户名：密码 
例：rsynctest:testrsync 
将文件属性修改为600（千万注意） 

3、启动rsync服务：rsync --daemon （rsync运行在tcp 873端口，可以通过netstat -an|grep LISTEN察看）。 

4、在B主机上（rsync客户机）上建立/etc/test文件，内容为A主机的密码，例： 
testsync 

5、用crontab建立脚本，例：0　21　*　*　1-5 rsync -vzrtp --progress --delete --password-file=/etc/test rsynctest@192.168.1.10::test /home/rsynctest  

rsync -vzrtopg --progress --delete --password-file=/etc/cufe.scrt /home/gpowersoft/ cufersync@10.13.6.77::cufebak 

rsync中的参数：v是verbose，z是压缩，r是recursive，tp都是保持文件原有属性如属主、时间  
的参数。--progress是指显示出详细的进度情况，--delete是指如果服务器端删除了这一文件，那么客户端也相应把文件删除，保持真正的一致。--password-file=/etc/test来指定密码文件，这样就可以在脚本中使  
用而无需交互式地输入验证密码了，这里需要注意的是这份密码文件权限属性要设得只有属主可读(600)。 
  
rsynctest@192.168.1.10中，rsynctest是指定密码文件中的用户名，192.168.1.10是A主机的IP地址::test是指模块名[test]，也就是在/etc/rsyncd.conf中自定义的名称。最后的/home/rsynctest是备份到本地的目录名。 
（也可以用-e ssh的参数建立起加密的连接，然后和scp中信任主机的办法一样如法炮制） 
（在上面实例中的rsynctest并不是真实的用户，可以根据自己需要文本定义，这也是使用rsync的一大好处） 

rsync的特点： 
特性如下：  

1、可以镜像保存整个目录树和文件系统。  
2、可以很容易做到保持原来文件的权限、时间、软硬链接等等。  
3、无须特殊权限即可安装。  
4、优化的流程，文件传输效率高。  
5、可以使用rcp、ssh等方式来传输文件，当然也可以通过直接的socket连接。  
6、支持匿名传输。  


