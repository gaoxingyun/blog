---
title: Spring框架总结
date: 2017-03-04 23:32:56
categories:
- 基础知识
tags:
- java
---

# Spring框架总结

###### spring优点

- 轻量级：Spring在大小和透明性方面绝对属于轻量级的，基础版本的Spring框架大约只有2MB。
- 控制反转(IOC)：Spring使用控制反转技术实现了松耦合。依赖被注入到对象，而不是创建或寻找依赖对象。
- 面向切面编程(AOP)： Spring支持面向切面编程，同时把应用的业务逻辑与系统的服务分离开来。
- 容器：Spring包含并管理应用程序对象的配置及生命周期。
- MVC框架：Spring的web框架是一个设计优良的web MVC框架，很好的取代了一些web框架。
- 事务管理：Spring对下至本地业务上至全局业务(JAT)提供了统一的事务管理接口。
- 异常处理：Spring提供一个方便的API将特定技术的异常(由JDBC, Hibernate, 或JDO抛出)转化为一致的、Unchecked异常。

###### spring框架模块

- Core module
- Bean module
- Context module
- Expression Language module
- JDBC module
- ORM module
- OXM module
- Java Messaging Service(JMS) module
- Transaction module
- Web module
- Web-Servlet module
- Web-Struts module
- Web-Portlet module

###### 核心容器

- BeanFactory模块是spring的核心，它是工厂模式的一种实现，使用控制反转（IOC）将应用的配置和依赖与应用实际代码分离
- XmlBeanFactory类是BeanFactory最常用的实现


###### Bean作用域

1. singleton：在Spring IOC容器中仅存在一个Bean实例，Bean以单实例的方式存在。spring默认配置。
2. prototype：一个bean可以定义多个实例。
3. request：每次HTTP请求都会创建一个新的Bean。该作用域仅适用于WebApplicationContext环境。
4. session：一个HTTP Session定义一个Bean。该作用域仅适用于WebApplicationContext环境.
5. globalSession：同一个全局HTTP Session定义一个Bean。该作用域同样仅适用于WebApplicationContext环境.

###### 取得spring管理的bean方式

1. 在初始化时保存ApplicationContext对象（适合非web程序）
2. 通过Spring提供的utils类获取ApplicationContext对象
3. 继承自抽象类ApplicationObjectSupport，WebApplicationObjectSupport或实现接口ApplicationContextAware
4. 通过Spring提供的ContextLoader取得

##### 自动装配模式（beans之间的依赖注入）

###### 方式

1. no：默认的方式是不进行自动装配，通过手工设置ref 属性来进行装配bean。
2. byName：通过参数名自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byName。之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
3. byType：通过参数的数据类型自动自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byType。之后容器试图匹配和装配和该bean的属性类型一样的bean。如果有多个bean符合条件，则抛出错误。
4. constructor：这个同byType类似，不过是应用于构造函数的参数。如果在BeanFactory中不是恰好有一个bean与构造函数参数相同类型，则抛出一个严重的错误。
5. autodetect：如果有默认的构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配

###### 局限性

1. 重写：你仍然需要使用 和< property>设置指明依赖，这意味着总要重写自动装配。
2. 原生数据类型:你不能自动装配简单的属性，如原生类型、字符串和类。
3. 模糊特性：自动装配总是没有自定义装配精确，因此，如果可能尽量使用自定义装配。

#### spring data

##### spring JDBC

###### JdbcTemplate

- JdbcTemplate类提供了许多方法，为我们与数据库的交互提供了便利。例如，它可以将数据库的数据转化为原生类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据库错误处理功能

##### spring事务

###### 事务实现方法

- 声明式事务（Transactional） 只能到方法级别（可将代码块独立为方法），此注解只应用到public方法上，其他方法上使用会被忽略。默认情况下，只有外部方法调用才会引起事务，类内部方法调用不会引起事务行为。
- 编程式事务（推荐使用TransactionalTemplate）

###### 事务自动提交

###### 事务回滚

指示spring事务管理器回滚一个事物的推荐方法是在此事务上下文内抛出异常，spring会捕获任何未经处理的异常，依据规则决定是否回滚事务。默认情况下，只有在异常为uncheck情况下才会回滚，check异常不会导致事务回滚。

###### spring事务传播级别

当spring中多个service的中方法被配置声明事务时，一个service调用另一个service时，事务的嵌套依赖与spring的事务传播级别配置。

- PROPAGATION_REQUIRED 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是 最常见的选择。
- PROPAGATION_SUPPORTS 支持当前事务，如果当前没有事务，就以非事务方式执行。
- PROPAGATION_MANDATORY 使用当前的事务，如果当前没有事务，就抛出异常。
- PROPAGATION_REQUIRES_NEW 新建事务，如果当前存在事务，把当前事务挂起。
- PROPAGATION_NOT_SUPPORTED 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- PROPAGATION_NEVER 以非事务方式执行，如果当前存在事务，则抛出异常。
- PROPAGATION_NESTED 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与 PROPAGATION_REQUIRED 类似的操作。

