---
title: sql语句学习
date: 2017-02-04 14:58:31
categories: 
- 总结
tags:
- sql
---

##
sql一直都很差，平时工作中又使用各种ORM框架，很少写sql，此次趁着最近没项目好好学习一下sql的使用。

## sql语法

#### sql查询

##### 关键字

- `SELECT` 查询
- `DISTINCT` 去重
- `AS` 别名
- `FROM` 数据来源
- `LEFT JOIN` 左外连接
- `RIGHT JOIN` 右外连接
- `FULL JOIN` 全外连接
- `WHERE` 数据筛选
- `BETWEEN` 数据区间筛选
- `IN` 与where连用，表名数据存在某范围里
- `LIKE` 模糊筛选
- `GROUP BY` 分组
- `HAVING` 分组后筛选，在group by之后执行
- `ORDER BY` 排序
- `UNION` 相同格式数据连接
- `EXIST` 存在，返回bool值
- `ANY` 只要有一条数据满足条件，整个条件成立
- `ALL` 对所有数据都满足条件，整个条件才成立

##### 常用统计函数

- `SUM()` 求和
- `AVG()` 求平均值
- `MIN()` 求最小值
- `MAX()` 求最大值
- `COUNT()` 统计返回数据个数，总是有结果

##### 关键字执行顺序

FROM -> WHERE -> SELECT -> GROUP BY -> HAVING -> ORDER BY

#### SQL分析过程

拿到一个需求，首先进行以下分析：
1. 确定所需要的数据表
2. 确定已知关联条件

#### 子查询

- select中：不常使用
- where中，子查询返回数据：单行单列，单行多列，多行单列。
- having中，子查询一般返回单行单列数据。
- from中，子查询相比多表查询，能避免笛卡尔积，提供效率。