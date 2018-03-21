---
title: oracle数据库使用
date: 2016-12-07 19:49:38
categories: 
- 工具
tags:
- 工具
- 数据库
---

## 重启数据库服务
1. 以oracle身份登录系统，命令：`su – oracle`
2. 进入Sqlplus控制台，命令：`sqlplus /nolog`
3. 以系统管理员登录，命令：`connect / as sysdba`
4. 启动数据库，命令：`startup`
5. 如果是关闭数据库，命令：`shutdown immediate /SHUTDOWN ABORT`
6. 退出sqlplus控制台，命令：`exit`
7. 进入监听器控制台，命令：`lsnrctl`
8. 启动监听器，命令：`start`
9. 退出监听器控制台，命令：`exit`
10. 重启数据库结束

## 查看数据库状态
- 查看监听状态 `lsnrctl status`

## 新建数据库
1. 首先，创建（新）用户：
        `create user username identified by password;`
        username：新用户名的用户名
        password: 新用户的密码
        也可以不创建新用户，而仍然用以前的用户，如：继续利用scott用户
2. 创建表空间：
         `create tablespace tablespacename datafile 'd:\data.dbf' size xxxm;`
         tablespacename：表空间的名字
         d:\data.dbf'：表空间的存储位置
         xxx表空间的大小，m单位为兆(M)
3. 将空间分配给用户：
        `alter user username default tablespace tablespacename;`
        将名字为tablespacename的表空间分配给username 
4. 给用户授权：
        `grant create session,create sequence,create table,unlimited tablespace to username;`
5. 然后再以自己创建的用户登录，登录之后创建表即可。
        `conn username/password;`

## 删除用户
- 删除用户和其数据，cascade表示删除数据，需此用户无连接。
        `drop user username cascade;`


## ORACLE数据库问题

#### 删除物化视图log表导致insert,delete,update报ORA-00942表或视图不存在

之前使用阿里yugong工具迁移数据库时自动创建了物化视图log表，之后同事清理不用的表时直接使用drop table删除了物化视图log表，第二天使用数据库发现select正常，insert、delete、update就报 ORA-00942: table or view does not exist。

##### 问题解决
- 查询dba_mview_logs中存在，而的log表不存在的对象
`select 'drop materialized view log on '||mvl.log_owner||'.'||mvl.master||';' cmd from dba_mview_logs mvl where not exists ( select table_name from dba_tables tab where tab.owner = mvl.log_owner and tab.table_name = mvl.log_table ) ;`
直接执行查询得到结果即可正确删除物化视图日志，解决问题。

参考[http://blog.itpub.net/27000195/viewspace-1400038](http://blog.itpub.net/27000195/viewspace-1400038)