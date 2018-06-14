---
title: jpa使用问题
date: 2017-07-27 16:22:53
categories: 
- 问题
tags:
- 问题
- java
- 数据库
---


#### mysql数据库表名默认小写问题

- jpa生成的sql语句表名是小写，但是实际表中是大写，配置@Table("TABLE_NAME")生成的sql仍然还是小写，造成sql执行出现表不存在异常

- 解决：
  1. 使用小写的数据库表名，这是mysql数据库推荐的形式。
  2. 在spring配置文件或java环境变量设置：
  ```
  spring.jpa.properties.lower_case_table_names: 0
  //环境变量 lower_case_table_names: 0
  ```

