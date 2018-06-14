---
title: mysql数据库使用
date: 2018-04-28 10:33:54
categories: 
- 工具
tags:
- 工具
- 数据库
---

## mysql数据库使用


#### 常用命令

- `mysql -h127.0.0.1 -P3306 -uuser -p123456` 登陆
- `show databases;` 查看库
- `use mydb;` 使用mydb库
- `show tables;` 查看表
- `source sql.sql` 执行sql.sql脚本文件



###### 问题

- mysql5.6创建funtion时失败，报 [Code: 1064, SQL State: 42000]  You have an error in your SQL syntax，这是由于数据库将;作为结束符，无法正确解析语句导致的，可以在sql命令行中，使用DELIMITER $$语句将结束符暂时变为$$符号，并在sql创建function语句最后加上$$符。

```sql
CREATE FUNCTION `FUN_SEQ`(SEQ VARCHAR(50)) RETURNS BIGINT(20)
BEGIN
     UPDATE SEQ_TABLE
     SET CURRENT_VALUE = CURRENT_VALUE + INCREMENT
     WHERE  SEQ_NAME=SEQ;
     RETURN FUN_SEQ_CURRENT_VALUE(SEQ);
END;

 -- [CREATE - 0 rows, 0.004 secs]  [Code: 1064, SQL State: 42000]  You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 5
```
改为
```shell
mysql> DELIMITER $$
mysql> CREATE FUNCTION `FUN_SEQ`(SEQ VARCHAR(50)) RETURNS BIGINT(20)
    -> BEGIN
    ->      UPDATE SEQ_TABLE
    ->      SET CURRENT_VALUE = CURRENT_VALUE + INCREMENT
    ->      WHERE  SEQ_NAME=SEQ;
    ->      RETURN FUN_SEQ_CURRENT_VALUE(SEQ);
    -> END$$
Query OK, 0 rows affected (0.00 sec)

# mysql命令行退出后，结束符号也会自动回到;
mysql> DELIMITER ; 
```


#### mysql主从配置

###### master
- 配置
```
# 日志文件名  
log-bin = mysql-bin  
  
# 主数据库端ID号  
server-id = 1
```

- 命令
```
mysql>grant replication slave on *.* to 'slave_account'@'%' identified by '123456';  
  
# 更新数据库权限  
mysql>flush privileges;

# 查看master状态，slave命令需用到
mysql> show master status;  
```

###### slave

- 配置

```
# 从数据库端ID号  
server-id =2 
```



- 命令

```
# 执行同步命令，设置主数据库ip，同步


帐号密码，同步位置  
mysql>change master to master_host='192.168.1.2',master_user='slave_account',master_password='123456',master_log_file='mysql-bin.000009',master_log_pos=196;  
  
# 开启同步功能  
mysql>start slave; 

# 检查从数据库状态
show slave status\G;  

# Slave_IO_Running及Slave_SQL_Running进程必须正常运行，即YES状态，否则说明同步失败。
```


- [https://blog.csdn.net/mycwq/article/details/17136001](https://blog.csdn.net/mycwq/article/details/17136001)

## 问题

- mysql关于错误InnoDB is limited to row-logging when transaction isolation level is READ COMMITTED or READ UNCOMMITTED.
暂时解决问题方法：
mysql> 
SET GLOBAL binlog_format = 'STATEMENT';
mysql> 
SET GLOBAL binlog_format = 'ROW';
mysql> 
SET GLOBAL binlog_format = 'MIXED';
永久解决问题方式：
在mysql的配置文件中，找到binlog_format=mixed，把前边的#去掉。
重启mysql服务！


- 设置最大连接数
```
# 设置全局最大连接数
set GLOBAL max_connections=200;
# 查看连接数
show processlist;
```