---
title: orm框架基础知识
date: 2017-03-04 20:36:42
categories:
- 框架
tags:
- 框架
- java
---


## Mybatis基础知识

#### 锁

- 在锁冲突较小的时候，使用乐观锁可以大大增加性能（减少类锁的开销），但是当并发较高，锁冲突特别严重的时候，由于乐观锁需要不断重试，反而会降低性能。另外，乐观锁不仅仅可以使用version字段，也可以使用时间戳，或数据库列字段等任何可以判断数据是否被修改的字段。乐观锁字段按具体业务选择，可增加并发性能，以version字段作为乐观锁，每次只能有一个事务更新成功，会造成大量操作失败，互不冲突的更新操作，宜使用不同的字段作为乐观锁。
##### 乐观锁

- 乐观锁是根据数据库的一个字段，在更新的时候判断数据有没有被改变。sql脚本类似如下：
```sql
update user set name='admin', version=version+1 where id='1' and version=1;
```



## 问题

- 报错  org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.builder.xml.IncompleteStatementException: Could not find parameter map java.util.Map  
解决：搜索文件*.xml 搜索词: parameterMap 然后将parameterMap 改为parameterType