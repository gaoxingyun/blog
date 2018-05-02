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

- `mysql -h127.0.0.1 -p3306 -uuser -p123456` 登陆
- `show databases;` 查看库
- `use mydb;` 使用mydb库
- `show tables;` 查看表
- `source sql.sql` 执行sql.sql脚本文件



###### 问题

- mysql5.6创建funtion时失败，报 [Code: 1064, SQL State: 42000]  You have an error in your SQL syntax，这是由于数据库将;作为结束复，无法正确解析语句导致的，可以在sql命令行中，使用DELIMITER $$语句将结束符暂时变为$$符号，并在sql创建function语句最后加上$$符。

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


