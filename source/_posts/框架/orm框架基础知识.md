---
title: orm框架基础知识
date: 2017-03-04 20:36:42
categories:
- 框架
tags:
- 框架
- java
---

## ORM框架总结

### Hibernate框架

##### 常用概念

###### session

- Hibernate session实际上是对java.sql.Connection的封装，一个session对应一个Connection

##### 持久化对象（Persistent Object）

持久化对象可以是普通的Javabeans,惟一特殊的是它们与（仅一个）Session相关联。JavaBeans在Hibernate中存在三种状态：
1. 临时状态(transient):当一个JavaBean对象在内存中孤立存在，不与数据库中的数据有任何关联关系时，那么这个JavaBeans对象就称为临时对象(Transient Object)。
2. 持久化状态(persistent):当一个JavaBean对象与一个Session相关联时，就变成持久化对象(Persistent Object)
3. 脱管状态(detached):在这个Session被关闭的同时，这个对象也会脱离持久化状态，就变成脱管状态(Detached Object)，可以被应用程序的任何层自由使用，例如可以做与表示层打交道的数据舆对象(Data Transfer Object)。

##### N + 1问题

- 一对多： 在一方，查找得到了 n 个对象，那么又需要将 n 个对象关联的集合取出，于是本来的一条 sql 查询变成了 n+1 条；
- 多对一： 在多方，查询得到了 m 个对象，那么也会将 m 个对象对应的 1 方的对象取出， 也变成了 m+1 ；

###### 解决

1. 设置@ManyToOne的fetch属性值为fetchType.LAZY，这种方式解决后，后面的n条sql语句按需而发。但是有个弊端，就是如果需要级联查询就无法获取级联对象了。
2. 设置@BatchSize(size=5)（该注解要加在类上面，跟@Entity在同一位置），这样发出的sql语句减少。这个设置在一定程度上提高了效率。
3. 在hql语句中使用用join fetch，事实上Criteria用的就是这种方法。

##### ID生成策略

##### 关联关系

###### 一对一

###### 一对多

###### 多对一

###### 多对多

##### 缓存

- 当数据库查询较多时，使用缓存会提高效率。当修改较多时，反而会降低效率

###### 一级缓存

- session缓存

###### 二级缓存

- sessionFactory缓存级别，开启后即使session关闭，也可以从缓存中查到数据

###### 三级缓存

- 查询缓存（sessionFactory级别），针对查询操作的缓存

#### 事务

###### 事务特性(ACID)

1. 原子性 Atomicity
2. 一致性 Consistence
3. 隔离性 Isolation
4. 持久性 Durability

##### 锁

数据库隔离级别设为读已提交，本应出现不可重复读问题，但是bibernate可以通过锁来避免此问题

###### 乐观锁
- 版本号，程序实现

###### 悲观锁
- 数据库实现

##### 事务嵌套

- JDBC 不支持嵌套事务，jdbc事务被限制在一个连接上
- JTA  支持嵌套事务，需要JAVA EE EJB容器。另外spring的声明式事务支持脱离JAVA EE使用JTA。


###### 数据库的事务嵌套

- mysql官方文档明确说明不支持事务嵌套，当开启一个新的事务时，任何当前的事务都会被隐式提交。

#### 使用方式

#### 与其他框架整合

##### 与spring框架整合

###### 优点

1. 由IOC容器来管理Hibernate的SessionFactory
2. 让Hibernate使用上Spring的声明式事务

###### 整合方式

1. 加入hibernate的jar包和配置文件hibernate.cfg.xml
2. 编写持久化类和对应的.hbm.xml文件
3. 加入spring的jar包和配置文件
4. 在spring配置文件中配置sessionFactory，事务等
5. 通过继承HibernateSupportDao得到HibernateTmplate获得session使用。(this.getHibernateTemplate().getSessionFactory())

###### Spring整合Hibernate事务配置

```xml
<!-- 事务管理器配置,单数据源事务 -->  
<bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">  
    <property name="sessionFactory" ref="sessionFactory" />  
</bean>  
<!-- 使用annotation定义事务 -->  
<tx:annotation-driven  transaction-manager="transactionManager"   proxy-target-class="true" />
```

## 数据库

### 事务

#### 事务并发问题


##### 脏读

- 读取到了其他事务未提交的数据

##### 不可重复读

- 在一个事物中两次读取结果不一致（读取到其他事物已经提交的数据） 

##### 幻读

- 在一个事物中两次读取数据条数不一致（其他事务添加删除了数据）


#### 事务隔离级别

##### 读未提交

- 会出现脏读，不可重复读，幻读问题

##### 读已提交

- 会出现不可重复读，幻读问题，oracle默认级别，数据库调优常用级别，并发隔离问题通过程序解决，如乐观锁

##### 可重复读

- 会出现幻读问题，mysql默认级别
- 数据库通过行锁实现

##### 串行化

- 避免所有并发隔离问题，效率极低，一般不使用

#### 更新丢失问题

##### 第一类更新丢失

- 事务A撤销时，把已经提交的事务B的更新数据覆盖了。任何隔离级别都不允许出现。

##### 第二类更新丢失

- 第2类丢失更新：事务A覆盖事务B已经提交的数据，造成事务B所做的操作丢失。读未提交，读提交会出现此类问题。（不可重复读带来的问题）
