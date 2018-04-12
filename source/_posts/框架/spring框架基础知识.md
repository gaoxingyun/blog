---
title: spring框架基础知识
date: 2017-03-04 23:32:56
categories:
- 框架
tags:
- 框架
- java
---

# Spring框架总结

## 框架概况

###### spring优点

- 轻量级：Spring在大小和透明性方面绝对属于轻量级的，基础版本的Spring框架大约只有2MB。
- 控制反转(IOC)：Spring使用控制反转技术实现了松耦合。依赖被注入到对象，而不是创建或寻找依赖对象。
- 面向切面编程(AOP)： Spring支持面向切面编程，同时把应用的业务逻辑与系统的服务分离开来。
- 容器：Spring包含并管理应用程序对象的配置及生命周期。
- MVC框架：Spring的web框架是一个设计优良的web MVC框架，很好的取代了一些web框架。
- 事务管理：Spring对下至本地业务上至全局业务(JTA)提供了统一的事务管理接口。
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


#### spring常用注解

- @Configuration把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。
- @Scope注解 作用域
- @Lazy(true) 表示延迟初始化
- @Service用于标注业务层组件、 
- @Controller用于标注控制层组件（如struts中的action）
- @Repository用于标注数据访问组件，即DAO组件。
- @Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。
- @Scope用于指定scope作用域的（用在类上）
- @PostConstruct用于指定初始化方法（用在方法上）
- @PreDestory用于指定销毁方法（用在方法上）
- @DependsOn：定义Bean初始化及销毁时的顺序
- @Primary：自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常
- @Autowired 默认按类型装配，如果我们想使用按名称装配，可以结合@Qualifier注解一起使用。如下：`@Autowired @Qualifier("personDaoBean")` 存在多个实例配合使用，如果类型存在多个实现类，会通过变量名ByName实现装配。
```java
// 以下都可成功装配
// 通常情况下@Autowired是通过byType的方法注入的，可是在多个实现类的时候，byType的方式不再是唯一，而需要通过byName的方式来注入，而这个name默认就是根据变量名来的。
@Autowired
private UserService userService1;
@Autowired
private UserService userService2;
@Autowired
@Qualifier(value = "userService2")
private UserService userService3;
```
- @Resource默认按名称装配，当找不到与名称匹配的bean才会按类型装配。
- @PostConstruct 初始化注解
- @PreDestroy 摧毁注解 默认 单例  启动就加载
- @Async异步方法调用


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

#### spring AOP

- AOP,面向切面编程，可以通过预编译方式（静态代理）和运行期（动态代理）实现在不修改源代码的情况下给程序动态统一添加功能的一种技术。

##### 代理机制

###### 静态代理

- 机制：静态织入。
- 原理：在编译期，切面直接以字节码的形式编译到目标字节码文件中。
- 优点：对系统无性能影响。
- 缺点：灵活性不够

###### 动态代理
- 介绍：JDK的动态代理主要涉及到java.lang.reflect包中的两个类：Proxy和InvocationHandler。其中 InvocationHandler是一个接口，可以通过实现该接口定义横切逻辑，在并通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编织在一起。JDK动态代理只能对实现了接口的类生成代理，而不能针对类 。
- 机制：JDK动态代理
- 原理：在运行期，目标类加载后，为接口动态生成代理类，将切面织入到代理类中。
- 优点：相对于静态AOP更加灵活。
- 缺点：切入的关注点需要实现接口。

###### 动态字节码生成

- 介绍：CGlib是一个强大的,高性能,高质量的Code生成类库。它可以在运行期扩展Java类与实现Java接口。 CGLIB是针对类实现代理的，主要对指定的类生成一个子类，并覆盖其中的方法， 因为是继承，所以不能使用final来修饰类或方法。和jdk代理实现不同的是，cglib不要求类实现接口。
- 机制：CGLIB
- 原理：在运行期，目标类加载后，动态构建字节码文件生成目标类的子类，将切面逻辑加入到子类中。
- 优点：没有接口也可以织入。
- 缺点：扩展类的实例方法为final时，则无法进行织入。（final类不可被继承）

###### 自定义类加载器

- 介绍：如果实现了一个自定义类加载器，在类加载到JVM之前直接修改某些类的方法，并将切入逻辑织入到这个方法里，然后将修改后的字节码文件交给虚拟机运行，更直接实现了AOP功能。
- 机制：Javassist
- 原理：在运行期，目标加载前，将切面逻辑加到目标字节码里。
- 优点：可以对绝大部分类进行织入，不会生成新的类。
- 缺点：代码中如果使用了其他类加载器，则这些类将不会被织入。

###### 字节码转换

- 介绍：使用Instrumentation，它是 Java 5 提供的新特性，使用 Instrumentation，开发者可以构建一个字节码转换器，在字节码加载前进行转换。
- 机制：Instrumentation，Javassist
- 原理：在运行期，所有类加载器加载字节码前进行拦截。
- 优点：可以对所有类进行织入。
- 缺点：

###### 参考
[http://www.cnblogs.com/xiaoxiao7/p/6057724.html](http://www.cnblogs.com/xiaoxiao7/p/6057724.html)

#### Spring定时任务

######

#### spring声明式缓存

###### 使用方式
1. 使用spring-boot并引入依赖
``` gradle
// cache
compile('org.springframework.boot:spring-boot-starter-cache')
compile('com.github.ben-manes.caffeine:caffeine')
```
2. 启用声明式缓存配置
```java
// 启用声明式缓存配置
@EnableCaching
@Configuration
public class CacheConfig {
}
```
3. 使用声明式缓存
```java
// 使用声明式缓存
@Cacheable(value = "transJnl", key = "#localSerial")
public TransJnl findTransJnlsByLocalSerial(String localSerial) {
	return transJnlMapper.findTransJnlsByLocalSerial(localSerial);
}
```
4. 更改application配置文件，设置缓存类型，并设置最大容量和过期时间
``` yml
spring:
 cache:
    type: CAFFEINE
    caffeine:
      spec: maximumSize=500,expireAfterWrite=5s
```

## Spring MVC

#### Spirng MVC 参数校验

- spring中使用validation校验框架（hibernate实现，JSR303标准）,不仅可以对VO进行校验，同时可以对DO进行校验。

###### 参数校验注解

- Bean Validation 中内置的 constraint

注解|运行时检查
-|:-
@Null	|被注释的元素必须为 null
@NotNull	|被注释的元素必须不为 null
@AssertTrue	|被注释的元素必须为 true
@AssertFalse	|被注释的元素必须为 false
@Min(value)	|被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@Max(value)	|被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@DecimalMin(value)	|被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@DecimalMax(value)	|被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@Size(max, min)	|被注释的元素的大小必须在指定的范围内
@Digits (integer, fraction)	|被注释的元素必须是一个数字，其值必须在可接受的范围内
@Past	|被注释的元素必须是一个过去的日期
@Future	|被注释的元素必须是一个将来的日期
@Pattern(value)	|被注释的元素必须符合指定的正则表达式


- Hibernate Validator 附加的 constraint

注解|运行时检查
-|:-
@Email	|被注释的元素必须是电子邮箱地址
@Length	|被注释的字符串的大小必须在指定的范围内
@NotEmpty	|被注释的字符串的必须非空
@Range	|被注释的元素必须在合适的范围内

###### 校验使用

1. 通过注解进行校验
```java


```


2. 通过代码进行校验

```java
    @Autowired
    private Validator validator;
```

