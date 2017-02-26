---
title: java基础知识总结
date: 2017-02-06 09:37:07
categories: 
- 总结
tags:
- java
---

## java语法

#### java关键字

- 八种基本数据类型
`boolean byte char short int float long double`
- 运算符
`+ - * / % & | && || instanceof(判断对象是否是某个类的实例)`
- 访问控制
`private default protect public`
- 类，方法，变量修饰符
`final static abstract native synchronized volatile transient(不参与序列化)`

#### java注释
- Java 有三种注释方式。前两种分别是 // 和 /* */，第三种被称作说明注释，它以 /** 开始，以 */结束。

## jdk类

#### jdk类命名

- java开头的包，属于java se
- javax开头的包，属于java ee，部分属于java se
- sun开头的包，属于jdk内部实现类，不是java api，不应调用

#### jdk异常类

###### 异常类结构

- java.lang.Object -> java.lang.Throwable -> java.lang.Exception -> java.lang.RuntimeException

###### 常见运行时异常

- `NullPointerException` 空指针异常
- `IndexOutOfBoundsException` 数组越界异常
- `ClassCastException` 类转换异常
- `ArithmeticException` 算术运算异常
- `BufferOverflowException` IO操作内存溢出异常 

#### jdk工具类

###### jdk容器类

- Collection 无序，允许重复
  - List 链表，可重复，有序
    - ArraryList
    - LinkList
    - Vector （线程安全）
    - Stack
  - Set 数学中的集合，不可重复，是否有序由实现决定。
    - AbstractSet
    - SortedSet
    - NavigableSet
    - TreeSet （用二叉树排序）
    - HashSet
    - LinkHashSet
    - EmumSet
  - Query 数据结构中的队列，先进先出。
    - AbstractQueue
    - PriorityQueue
    - Deque
- Map key-value结构，是否有序由实现决定
  - AbstractMap
  - SortedMap
  - NavigableMap
  - TreeMap （用二叉树排序）
  - HashMap
  - LinkedHashMap
  - HashTable
  - EmumMap
  - WeakHashMap
  - IdentityHashMap
  - ConcurrentHashMap

#### Java并发

- java.util.concurrent包

###### 并发容器

- BlockingQueue 阻塞队列接口
  - ArrayBlockingQueue
  - DelayQueue
  - LinkedBlockingQueue
  - PriorityBlockingQueue
  - SynchronousQueue
- BlockingDeque 双端阻塞队列接口
  - LinkedBlockingDeque
- ConcurrentLinkedQueue
- ConcurrentLinkedDeque
- ConcurrentMap 并发Map
  - ConcurrentHashMap

###### 原子对象

- AtomicBoolean
- AtomicInteger
- AtomicIntegerArray
- AtomicIntegerFieldUpdater
- AtomicLong
- AtomicLongArray
- AtomicLongFieldUpdater
- AtomicMarkableReference
- AtomicReference
- AtomicReferenceArray
- AtomicReferenceFieldUpdater
- AtomicStampedReference


## java多线程

#### java线程池


#### 多线程特点

###### 多线程优点

1. 充分利用硬件资源
2. 结构优雅
3. 简化异步处理

###### 多线程缺点

1. 线程安全问题
2. 活跃性问题（等待锁甚至死锁的问题）
3. 性能问题

#### 线程同步与互斥

#### 线程安全

###### 线程安全概述

- 线程安全：多个线程访问同一个对象时，如果不用考虑这些线程在运行时环境下的调度和交替执行，也不需要进行额外的同步或者在调用方进行任何其他的协调操作，调用这个对象的行为都可以得到正确的结果，那这个对象就是线程安全的。

###### 线程安全分类

- 不可变
- 绝对线程安全
- 相对线程安全
- 线程兼容
- 线程对立

###### 解决机制

- 加锁
  - 使用synchronized关键字，使代码串行化运行
  - 加锁应考虑性能问题，将不影响共享状态的代码从synchronized中分离出去
  - 保证所有线程的可见性（能看到最新值）
- 不共享状态
  - 使用无状态对象
  - 使用单线程
- 不可变对象
  - 使用final修饰的对象
 
###### 同步的实现

1. synchronized
2. wait,notify

###### 关键字

1. synchronized 保证可见性及原子性
   - 需获取到对象锁才能进入同步方法或代码块进行操作
   - 修饰方法，对象锁即为方法所在的对象
   - 修饰代码块，对象锁即为代码块
2. volatile 保证可见性，但不保证原子性，因此并不是线程安全的，但是代价较小的同步策略。

##### 死锁

###### 产生条件

1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

###### 处理死锁办法

1. 预防死锁。
　　这是一种较简单和直观的事先预防的方法。方法是通过设置某些限制条件，去破坏产生死锁的四个必要条件中的一个或者几个，来预防发生死锁。预防死锁是一种较易实现的方法，已被广泛使用。但是由于所施加的限制条件往往太严格，可能会导致系统资源利用率和系统吞吐量降低。
2. 避免死锁。
　　该方法同样是属于事先预防的策略，但它并不须事先采取各种限制措施去破坏产生死锁的的四个必要条件，而是在资源的动态分配过程中，用某种方法去防止系统进入不安全状态，从而避免发生死锁。
3. 检测死锁。
　　这种方法并不须事先采取任何限制性措施，也不必检查系统是否已经进入不安全区，此方法允许系统在运行过程中发生死锁。但可通过系统所设置的检测机构，及时地检测出死锁的发生，并精确地确定与死锁有关的进程和资源，然后采取适当措施，从系统中将已发生的死锁清除掉。
4. 解除死锁。
　　这是与检测死锁相配套的一种措施。当检测到系统中已发生死锁时，须将进程从死锁状态中解脱出来。常用的实施方法是撤销或挂起一些进程，以便回收一些资源，再将这些资源分配给已处于阻塞状态的进程，使之转为就绪状态，以继续运行。死锁的检测和解除措施，有可能使系统获得较好的资源利用率和吞吐量，但在实现上难度也最大。



## java内存模型
