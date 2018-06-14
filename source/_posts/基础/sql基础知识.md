---
title: sql基础知识
date: 2017-02-04 14:58:31
categories: 
- 基础
tags:
- 基础
- 数据库
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
- `JOIN ON` 连接关联条件，在JOIN之前执行，而WHERE在JOIN之后执行
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

#### 常用函数
##### 常用统计函数

- `SUM()` 求和
- `AVG()` 求平均值
- `MIN()` 求最小值
- `MAX()` 求最大值
- `COUNT()` 统计返回数据个数，总是有结果


##### 精度函数

- `CAST()` 保留小数位数


##### 空值函数

- `NVL()`

##### 常用函数示例
- `CAST(1.234 AS DECIMAL(10,2))` 保留精度位函数，例子保留两位小数，包括小数共十位数。
- `NVL(num, 0)`

#### 控制语句

##### CASE

###### 简单CASE语句

```sql
CASE sex
WHEN '1' THEN '男'
WHEN '2' THEN '女'
ELSE '其他' END
```

###### CASE搜索语句

```sql
CASE WHEN sex = '1' THEN '男' 
WHEN sex = '2' THEN '女' 
ELSE '其他' END 
```

###### CASE语句示例

- 统计数量
```sql
-- CASE结合SUM统计数量
-- 统计年龄分组男性数量和女性数量
SELECT 
-- 男性
SUM(CASE WHEN sex = '1' THEN 1
ELSE 0 END,)
-- 女性
SUM(CASE WHEN sex = '0' THEN 1
ELSE 0 END)
GROUP BY age;
```

##### 关键字执行顺序

FROM -> ON -> JOIN -> WHERE -> SELECT -> GROUP BY -> HAVING -> ORDER BY

#### SQL分析过程

拿到一个需求，首先进行以下分析：
1. 确定所需要的数据表
2. 确定已知关联条件

#### 子查询

- select中：不常使用
- where中，子查询返回数据：单行单列（=），单行多列（多个字段相同 (a,b) = ()），多行单列（in,any,all）。
- having中，子查询一般返回单行单列数据。
- from中，子查询相比多表查询，能避免笛卡尔积，提高效率。


## 数据库基础知识

#### 关系运算（共五种）
1. 并：R,S具有相同的关系模式（元素相同，结构相同），记为R U S,返回由R或者S元组构成的集合组成
2. 差：R,S具有相同的关系模式（元素相同，结构相同），记为R-S，右属于R但不属于S的元组组成
3. 广义笛卡尔积：R×S由n目和m目的关系R,S组成一个（n+m）列的元组集合，若R有K1个元组，S有K2个元组，则R×S有K1*K2个元组
4. 投影（π）：从关系的垂直方向开始运算，选择关系中的若干列组成新的列。
5. 选择（σ）：选择从关系的水平方向进行元算，选择满足给定条件的元组组成新的关系。


#### 


## sql问题

######

- union all中不能使用多个order by子句，只能使用一个。 [关于union all中使用多个order by 子句引起的问题 ](http://blog.chinaunix.net/uid-20449297-id-1676810.html)
