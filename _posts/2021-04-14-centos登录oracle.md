---

layout: post
title:  "CentOS7登录Oracle"
categories: [实施工程]
tags: [CentOS7]

---

su - oracle  
sqlplus /nolog  
conn / as sysdba    

这样就进数据库dba了  

查询oracle版本：`select * from v$version;`  

查询表空间：

sqlplus / as sysdba连接到oracle   
startup 启动数据库  
查看表空间  
select *from DBA_TABLESPACES  
or  
selelct *from USER_TABLESPACES    

---

Oracle架构一大特点：软件系统硬件化思维  CDB（容器数据库）PDB（可拔插数据库），PDB可以插在CDB上，就像U盘插在电脑上一样，

sqlplus   

创建新用户： CREATE USER OT IDENTIFIED BY Oracl1234   

授权： GRANT CONNECT，RESIURCE，DBA TO OT；  

登录新账号：CONNECT ot@orcl  

执行SQL文件：@+路径  

全局数据库名 = 数据库名 + 数据库域名  

数据库服务名： 如果数据库有域名，服务名 = 全局数据库名；如果没有数据库域名，服务名 = 数据库名  

数据库实例名（instance_name）与ORACLE_SID区别：instance_name是数据库参数，ORACLE_SID是操作系统的环境变量，数据库和操作系统之间交互用instance_name，从操作系统角度访问实例名（instance_name)，必须通过ORACLE_SID，且实例名（instance_name）和ORACLE_SID的值必须一致，否则，Unix和Win系统都会报错，Unix：“ORACLE not available”；Win：“TNS：协议适配器错误”。

连接CDB库：sqlplus / as sysdba  

连接PDB库：sqlplus pdb_user/pdb_password @pdb_service  

查看是否开启了容器：select name ,cdb from v$containers  

查询容器数据库所有数据文件：select con_id,file_name from cdb_data_files order by 1;    



详细教程：<https://www.w3cschool.cn/oraclejc/>

（未完待续）  