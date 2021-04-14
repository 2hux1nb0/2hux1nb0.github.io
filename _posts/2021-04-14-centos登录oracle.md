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



（未完待续）  