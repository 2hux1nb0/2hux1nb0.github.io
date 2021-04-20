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

####  对全局数据库的一些操作：

1. *#Step1.* *查看SGA的大小：因为DB_CACHE_SIZE的size受SGA的影响*
2. SQL> show parameter sga_max_size;
3. NAME                     TYPE    VALUE
4. ------------------------------------ ----------- ------------------------------
5. sga_max_size                 big integer 2G
6. 
7. *#Step2.* *查看show parameter shared_pool_size的大小*
8. SQL> show parameter shared_pool_size;                   NAME                     TYPE    VALUE
9. ------------------------------------ ----------- ------------------------------
10. shared_pool_size             big integer 0
11. 
12. *#Step3.* *计算DB_CACHE_SIZE的大小：shared_pool_size* *+* *db_cache_size* *=* *SGA_MAX_SIZE* *** *70**%*
13. 
14. *#Step4.* *修改DB_CACHE_SIZE的大小*
15. SQL> alter system set db_cache_size=1433M scope=spfile sid='demo';
16.  
17. System altered.
18.  
19. SQL> conn sys /as sysdba
20. Enter password: ********
21. Connected.
22. SQL> shutdown immediate
23. Database closed.
24. Database dismounted.
25. ORACLE instance shut down.
26. SQL> startup
27. ORACLE instance started.
28.  
29. Total System Global Area 2147483648 bytes
30. Fixed Size          2022144 bytes
31. Variable Size         503317760 bytes
32. Database Buffers     1627389952 bytes
33. Redo Buffers           14753792 bytes
34. Database mounted.
35. Database opened.
36.  
37. SQL> show parameter db_cache_size



详细教程：<https://www.w3cschool.cn/oraclejc/>  

Oracle的SQL语法并无多大不同和难度，与常规sql语法差不多，只是一个熟练度的问题。

（未完待续）  