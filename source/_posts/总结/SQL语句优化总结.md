---
title: SQL语句优化总结
date: 2017-09-28 17:44:47
categories: 
- 总结
tags:
- 总结
- 数据库
- SQL
---

### mysql优化实践
###### 博客
- [https://yq.aliyun.com/articles/183749?spm=5176.100239.bloglist.157.m7bEPS](https://yq.aliyun.com/articles/183749?spm=5176.100239.bloglist.157.m7bEPS)
- [http://blog.csdn.net/xifeijian/article/details/19773795](http://blog.csdn.net/xifeijian/article/details/19773795)

#### explain
[http://blog.csdn.net/xiaolyuh123/article/details/53116168](http://blog.csdn.net/xiaolyuh123/article/details/53116168)


#### 联合查询驱动表

- 小结果集驱动大结果集


#### mysql临时表
[http://blog.csdn.net/xiaolyuh123/article/details/53286033](http://blog.csdn.net/xiaolyuh123/article/details/53286033)
- 以下情况会产生临时表
 1. UNION查询；
 2. 用到TEMPTABLE算法或者是UNION查询中的视图；
 3. ORDER BY和GROUP BY的子句不一样时；
 4. 表连接中，ORDER BY的列不是驱动表中的；
 5. DISTINCT查询并且加上ORDER BY时；
 6. SQL中用到SQL_SMALL_RESULT选项时；
 7. FROM中的子查询；
 8. 子查询或者semi-join时创建的表；

- 以下几种情况下，会创建磁盘临时表：
 1. 数据表中包含BLOB/TEXT列；
 2. 在 GROUP BY 或者 DSTINCT 的列中有超过 512字符 的字符类型列（或者超过 512字节的 二进制类型列，在5.6.15之前只管是否超过512字节）；
 3. 在SELECT、UNION、UNION ALL查询中，存在最大长度超过512的列（对于字符串类型是512个字符，对于二进制类型则是512字节）；
 4. 执行SHOW COLUMNS/FIELDS、DESCRIBE等SQL命令，因为它们的执行结果用到了BLOB列类型。

#### mysql索引
[http://blog.csdn.net/xiaolyuh123/article/details/53116193](http://blog.csdn.net/xiaolyuh123/article/details/53116193)


#### 
- union总是产生临时表

#### sql优化建议
- 
