---
title: java基础知识
date: 2017-02-06 09:37:07
categories: 
- 基础
tags:
- 基础
- java
---

#### java语法

###### java常用关键字

- 八种基本数据类型
`boolean 1 byte 1 char 2 short 2 int 4 float 4 long 8 double 8`
- 运算符
`+ - * / % & | && || instanceof(判断对象是否是某个类的实例)`
- 访问控制
`private default protect public`
- 类，方法，变量修饰符
`final static abstract native synchronized volatile transient(不参与序列化)`

###### Java关键字列表 (依字母排序 共50组)
```java
abstract, assert, boolean, break, byte, case, catch, char, class, const（保留关键字）, continue, default, do, double, else, enum, extends, final, finally, float, for, goto（保留关键字）, if, implements, import, instanceof, int, interface, long, native, new, package, private, protected, public, return,short, static, strictfp, super, switch, synchronized, this,throw, throws, transient, try, void, volatile, while 
```
###### java保留字列表 (依字母排序共14组)，Java保留字是指现有Java版本尚未使用，但以后版本可能会作为关键字使用
```java
byValue, cast, false, future, generic, inner,operator, outer, rest, true, var, goto（保留关键字）, const（保留关键字）, null
```

#### java定义

##### java数组

###### java二维数组

Java语言中，由于把二维数组看作是数组的数组，数组空间不是连续分配的，所以不要求二维数组每一维的大小相同。（C中由于内存连续，每维必须相同，且只有第一维可省略）
```java
int[][] temp1 = new int[1][2];
int[][] temp2 = new int[5][]
int[][] temp3 = new int[][]{{1,2},{3,4,5}};
int[][] temp4 = {{1,2},{3,4,5}};
```

#### java注释
- Java 有三种注释方式。前两种分别是 // 和 /* */，第三种被称作说明注释，它以 /** 开始，以 */结束。

#### java机制

##### 重写
方法的重写（override）两同两小一大原则：
1. 方法名相同，参数类型相同
2. 子类返回类型小于等于父类方法返回类型，
3. 子类抛出异常小于等于父类方法抛出异常，
4. 子类访问权限大于等于父类方法访问权限。

##### 值传递和引用传递

- 值传递传参数时，形式参数是实际参数的一份拷贝（非基本类型指向同一块内存地址，即其值相同，而其本身的地址不同）

##### java反射
JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。
Java反射机制主要提供了以下功能： 
- 在运行时判断任意一个对象所属的类；
- 在运行时构造任意一个类的对象；
- 在运行时判断任意一个类所具有的成员变量和方法；
- 在运行时调用任意一个对象的方法；
- 生成动态代理。


##### java范型

###### 语法

- K 表示键，V 表示值，E 表示异常或错误，T 表示一般意义上的数据类型， T1，T2代表不同数据类型
- <T extends Number> 接受Number子类  <T supper Number> 接受Number父类
- <T> 声明一个范型

###### 优点

1. 类型安全，将类型检查从运行时变到编译时
2. 消除代码中类型强制类型转换，增强可读性
3. 为较大优化带来可能

###### 类型擦除

1. java使用范型加上的类型参数，会被编译器在编译时候去掉。这个过程被称为类型擦除。

###### 代码示例

```java
ArraryList<String> list = new ArraryList(); // 正确
ArraryList list = new ArraryList<String>(); // 无效果，因为类型检查是在编译时完成的，new ArraryList()只是在内存开辟空间，其可以存储任意类型对象，而真正涉及类型检查的是它的引用。
```
```java
 public <T extends Object> T getObject(Class<T> c) throws IllegalAccessException, InstantiationException {
        T t = c.newInstance();
        return t;
    }
```

###### 问题

- 不能抛出也不能捕获范型类的对象，因为异常都是在运行时被捕获和抛出的，而在编译时，所有范型信息被清除。
- 不允许创建范型数组。
- 静态方法和静态变量不能使用范型，因为范型实例化是在定义对象时指定的，而静态变量方法不需对象调用。

#### Java注解

##### 分类

###### 根据注解使用方法和用途分类
- JDK 内置系统注解
- 元注解 用来修饰注解的注解
- 自定义注解

##### JDK内置系统注解

###### 重写方法
- @Override

###### 标记过时
- @Deprecated

###### 忽略警告
- @SuppressWarnings

##### 元注解
- 元注解的作用就是用来注解其它注解，主要有四种元注解

###### @Target
- 注明了注解修饰的对象的类型，取值（枚举类型 ElementType）有：
  - CONSTRUCTOR: 修饰构造器
  - FIELD:修饰域
  - LOCAL_VARIABLE:修饰局部变量
  - METHOD:修饰方法
  - PACKAGE:修饰包
  - PARAMETER:修饰参数
  - TYPE:修饰类、接口(包括注解类型) 或enum声明

###### @Retention
- 定义了该Annotation被保留的时间长短，取值（RetentionPoicy）有：
  - SOURCE: 源文件有效
  - CLASS: class文件中有效
  - RUNTIME: 运行时有效


###### @Documented
- 用于描述其它类型的Annotation应该被作为被标注的程序成员的公共API

###### @Inherited
- 阐述了某个被标注的类型是被继承的。如果一个使用了 @Inherited 修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。
  - 注意：@Inherited annotation类型是被标注过的class的子类所继承。类并不从它所实现的接口继承annotation，方法并不从它所重载的方法继承annotation。
    1. 如果子类继承父类，并且重写了父类中的带有注解的方法，那么父类方法上的注解是不会被子类继承的。
    2. 如果子类继承父类，但是没有重写父类中带有注解的方法，那么父类方法上的注解会被子类继承，就是说在子类中可以得到父类方法上的注解。
  - 即使在注解中没有 @Inherited 也会满足上面情况，但是如果该注解是用来修饰类的，则不满足。


##### 自定义注解

###### 定义注解格式
- public @interface 注解名 {定义体}

###### 注解参数的可支持数据类型
- 所有基本数据类型（int,float,boolean,byte,double,char,long,short)
- String类型
- Class类型
- enum类型
- Annotation类型
- 以上所有类型的数组

###### 其他约束
- 对于注解中的参数，只能用public或默认(default)这两个访问权修饰.例如,String value();如果只有一个参数成员,最好把参数名称设为"value"
- 注解元素必须有确定的值，要么在定义注解的默认值中指定，要么在使用注解时指定，非基本类型的注解元素的值不可为null。

###### 解析注解
```java

// 定义注解
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Annonation{
    String value() default "defalut value";
}
// 使用注解
@Annonation("my annonation")
class  MyClass{}
// 解析注解
Class c = Class.forName("");
Annonation annonation = (Annonation)c.getAnnotation(Annonation.class);
System.out.println(annonation.value());
```


#### Java特性

##### 面向对象三大特性

###### 继承
###### 封装
###### 多态
- 定义： 指允许不同类的对象对同一消息作出响应。即同一消息可以根据发送对象的不同而采用不同的行为。（发送消息即函数调用）
- 条件：有继承，有重写，有父类引用指向子类对象
- 实现技术： 动态绑定

#### jdk类

##### jdk类命名

- java开头的包，属于java se
- javax开头的包，属于java ee，部分属于java se
- sun开头的包，属于jdk内部实现类，不是java api，不应调用

##### jdk异常类

###### 异常类结构

- java.lang.Object -> java.lang.Throwable -> java.lang.Exception -> java.lang.RuntimeException

###### 常见运行时异常

- `NullPointerException` 空指针异常
- `IndexOutOfBoundsException` 数组越界异常
- `ClassCastException` 类转换异常
- `ArithmeticException` 算术运算异常
- `BufferOverflowException` IO操作内存溢出异常 

##### jdk工具类

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
  - HashMap (线程不安全，数组+链表实现)
  - LinkedHashMap
  - HashTable
  - EmumMap
  - WeakHashMap
  - IdentityHashMap
  - ConcurrentHashMap

##### Java并发类

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

#### 多线程基础

###### 多线程实现方法

1. 继承Thread方法
2. 实现Runable接口
3. 实现Callable接口，并通过Feture获得线程执行结果


###### 多线程方法

- start 方法使一个线程就绪，当线程执行时会执行run方法
- run 方法只是一个普通方法，执行不会新增加线程

###### ThreadLocal类

- ThreadLocal类用于创建一个线程本地变量，InheritableThreadLocal中子线程会继承父线程的ThreadLocal值，但继承后同样是独立拥有的副本。

#### java线程池

- Java通过Executors提供以下四种线程池

##### 线程池类型

###### newCachedThreadPool
- 创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。

###### newFixedThreadPool
- 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。

###### newScheduledThreadPool
- 创建一个定长线程池，支持定时及周期性任务执行。

###### newSingleThreadExecutor
- 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

##### 线程池参数

```java
 /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```
##### 线程池参数详解
- int corePoolSize 核心线程数量
- int maximumPoolSize 最大线程数量
- long keepAliveTime 非核心线程保持时间，超出时间，销毁线程
- TimeUnit unit keepAliveTime时间单位
- BlockingQueue<Runnable> workQueue 工作阻塞队列，处理不了的任务，先放到工作队列中，可创建多种阻塞队列
- ThreadFactory threadFactory 现程创建工厂
- RejectedExecutionHandler handler 拒绝处理器，有多种拒绝策略

###### RejectedExecutionHandler拒绝处理器

- CallerRunsPolicy 直接运行这个任务的run方法。但是，请注意并不是在ThreadPoolExecutor线程池中的线程中运行，而是直接调用这个任务实现的run方法
- AbortPolicy 在任务被拒绝后会创建一个RejectedExecutionException异常并抛出。这个处理过程也是ThreadPoolExecutor线程池默认的RejectedExecutionHandler实现
- DiscardPolicy 将会默默丢弃这个被拒绝的任务，不会抛出异常，也不会通过其他方式执行这个任务的任何一个方法，更不会出现任何的日志提示
- DiscardOldestPolicy 会检查当前ThreadPoolExecutor线程池的等待队列。并调用队列的poll()方法，将当前处于等待队列列头的等待任务强行取出，然后再试图将当前被拒绝的任务提交到线程池执行



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
  - 使用无状态对象（常用的贫血模式就是无状态对象，但是从面向对象角度，贫血模式是一种不良设计）
  - 使用局部对象（即在方法内部创建对象，保证对象不被其他线程共享）
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
2. volatile 保证可见性，但不保证原子性，因此并不是线程安全的，但是代价较小的同步策略。另外volatile可以禁止指令重排序。

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

### JVM知识

#### 内存模型

##### Java内存模型

###### 内存分类
- 主内存   所有线程共享
- 工作内存 线程私有
- 执行引擎 实际代码执行的位置

###### 内存使用
-  Java内存模型的主要目标是定义程序中各个变量的访问规则，即在虚拟机中将变量存储到内存和从内存中取出变量这样的底层细节。此处的变量主要是指共享变量，存在竞争问题的变量。Java内存模型规定所有的变量都存储在主内存中，而每条线程还有自己的工作内存，线程的工作内存中保存了该线程使用到的变量的主内存副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量（根据Java虚拟机规范的规定，volatile变量依然有共享内存的拷贝，但是由于它特殊的操作顺序性规定——从工作内存中读写数据前，必须先将主内存中的数据同步到工作内存中，所有看起来如同直接在主内存中读写访问一般，因此这里的描述对于volatile也不例外）。不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值得传递均需要通过主内存来完成。

###### 内存操作指令
1. luck（锁定）：作用于主内存的变量，它把一个变量标示为一条线程独占的状态。
2. unlock（解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。
3. read（读取）：作用于主内存的变量，它把一个变量的值从主内存传输到工作内存中，以便随后的load动作使用。
4. load（载入）：作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。
5. use（使用）：作用于工作内存的变量，它把工作内存中的一个变量的值传递给执行引擎，每当虚拟机遇到一个需要使用到变量的值得字节码指令时将会执行这个操作。
6. assign（赋值）：作用于工作内存的变量，它把一个从执行引擎接收到的值赋给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
7. store（存储）：作用于工作内存的变量，它把工作内存中的一个变量的值传递到主内存中，以便随后的write操作使用。
8. write（写入）：作用于主内存的变量，它把store操作从工作内存中得到的变量值放入主内存的变量中。

###### 参考
[http://blog.csdn.net/vking_wang/article/details/8574376#t2](http://blog.csdn.net/vking_wang/article/details/8574376#t2)

#### 内存分类
##### 运行时数据区

###### 线程隔离

- 虚拟机栈
- 本地方法栈
- 程序计数器

###### 线程共享

- 方法区(非堆)
- 堆

##### 其他内存区域

###### 堆外内存

- JDK1.4加入的NIO，使用Native函数库直接分配堆外内存，并通过DirectByteBuffer对象进行引用操作内存。（避免在Java堆和Native堆中来回复制数据）

#### 类加载器

##### 类加载器分类

- bootstrap classloader －引导（也称为原始）类加载器，它负责加载Java的核心类。 
- extension classloader －扩展类加载器，它负责加载JRE的扩展目录（JAVA_HOME/jre/lib/ext或者由java.ext.dirs系统属性指定的）中JAR的类包。 
- system classloader －系统（也称为应用）类加载器，它负责在JVM被启动时，加载来自在命令java中的-classpath或者java.class.path系统属性或者CLASSPATH*作系统属性所指定的JAR类包和类路径。

##### 类加载机制（委托加载机制）

- 当一个类需要被加载时，它会去请求调用它的父加载器，从最顶级的父加载器向子级寻找。原始类加载器 -> 扩展类加载器 -> 应用加载器顺序寻找，此机制可以防止用户实现的类覆盖掉JavaApi。eg：自定义一个java.lang.Long类，运行时实际执行的是JavaApi中的Long。

#### 垃圾回收

##### 回收算法

两个最基本的java回收算法：复制算法和标记清理算法
- 复制算法：两个区域A和B，初始对象在A，继续存活的对象被转移到B。此为新生代最常用的算法
- 标记清理：一块区域，标记要回收的对象，然后回收，一定会出现碎片，那么引出
- 标记-整理算法：多了碎片整理，整理出更大的内存放更大的对象

##### java分代回收

分代的垃圾回收策略，是基于这样一个事实：不同的对象的生命周期是不一样的。因此，不同生命周期的对象可以采取不同的收集方式，以便提高回收效率。

###### 分代

1. 新生代（Young Generation）
  - 回收机制 ：因为对象数量少，所以采用复制回收。
  - 组成区域 ：由1个Eden（伊甸园）区和2个Survivor（幸存者）区构成，同一时间的两个Survivor区，一个用来保存对象，另一个是空的；每次进行Young代垃圾回收的时候，就把Eden，From中的可达对象复制到To区域中，一些生存时间长的就复制到了老年代，接着清除Eden，From空间，最后原来的To空间变为From空间，原来的From空间变为To空间。
  - 对象来源 ：绝大多数对象先分配到Eden区，一些大的对象会直接被分配到Old代中。
  - 回收频率 ：因为Young代对象大部分很快进入不可达状态，因此回收频率高且回收速度快。
2. 老年代（Old Generation）
  - 回收机制 ：采用标记压缩算法回收。
  - 对象来源 ：1.对象大直接进入老年代。2.Young代中生存时间长的可达对象
  - 回收频率 ：因为很少对象会死掉，所以执行频率不高，而且需要较长时间来完成。
3. 永久代（Permanent Generation）（Java8去除永久代，改用元空间）
  - 用途 ：用来装载Class，方法，静态变量等信息，垃圾回收不会发生在永久代,如果永久代满了或超出了临界值,会触发jvm的完全垃圾回收(full gc)对其进行回收
  - 对象来源 ：eg：对于像Hibernate，Spring这类喜欢AOP动态生成类的框架，往往会生成大量的动态代理类，因此需要更多的Permanent代内存。
  - 回收频率 ：不会被回收
  
###### 触发垃圾回收

由于对象进行了分代处理，因此垃圾回收区域、时间也不一样。GC有两种类型：Minor GC和Full GC。
- Minor GC：一般情况下，当新对象生成，并且在Eden申请空间失败时，就会触发Minor GC，对Eden区域进行GC，清除非存活对象，并且把尚且存活的对象移动到Survivor区。然后整理Survivor的两个区。这种方式的GC是对年轻代的Eden区进行，不会影响到年老代。因为大部分对象都是从Eden区开始的，同时Eden区不会分配的很大，所以Eden区的GC会频繁进行。因而，一般在这里需要使用速度快、效率高的算法，使Eden去能尽快空闲出来。
- Full GC：对整个堆进行整理，包括Young、Tenured和Perm。Full GC因为需要对整个对进行回收，所以比Minor GC要慢，因此应该尽可能减少Full GC的次数。在对JVM调优的过程中，很大一部分工作就是对于Full GC的调节。有如下原因可能导致Full GC：
  1. 老年代（Tenured）被写满。
  2. 永久代（Perm）被写满。
  3. System.gc()被显示调用。

##### 垃圾回收器

###### 分类

1. 串行回收器：串行回收器是最简单的一个，主要面对单线程环境和比较小的堆，这个回收器工作的时候会将所有应用线程全部冻结，就这一点而言就注定它不适合应用于服务端。（可以打开-XX:+UseSerialGC这个JVM参数来启用它。） 
2. 并行回收器：这是JVM的默认回收器，它使用多个线程来扫描及压缩堆，但缺点在于不管执行的是minor GC还是full GC它都会暂停应用线程。并行回收器最适合那些可以容许暂停的应用，它试图减少由回收器所引起的CPU开销。 (最大吞吐量优先，可通过参数调整目标最大运行暂停时间)
3. CMS回收器：这个算法在两种情况下会进入一个”stop the world”的模式：当进行根对象的初始标记的时候 （老生代中线程入口点或静态变量可达的那些对象）以及当这个算法在并发运行的时候应用程序改变了堆的状态使得它不得不回去再次确认自己标记的对象都是正确的。这个算法的弊端在于，如果回收器需要将年轻的对象提升到年老代中，而刚好这个时候年老代已经没有多余的空间了，它就会进行一次“stop the world”模式下的full GC，为避免这种情况发生，一个方法是增加年轻代和年老代的大小（或者增加整个堆的大小），另一个方法是给回收器分配一些后台线程以便与对象分配的速度进行赛跑。（可以打开-XX:+UseConcMarkSweepGC来启用它） 
4. G1回收器：G1（ Garbage first）回收器在JDK 7update 4中首次引入，它的设计目标是能更好地支持大于4GB的堆。G1回收器将堆分为多个区域，大小从1MB到32MB不等，并使用多个后台线程来扫描它们。G1回收器会优先扫描那些包含垃圾最多的区域，这正是它的名字的由来（Garbage first）。这一策略减少了后台线程还未扫描完无用对象前堆就已经用光的可能性，而那种情况回收器就必须得暂停应用，这就会导致STW回收。G1的另一个好处就是它总是会进行堆的压缩，而CMS回收器只有在full GC的时候才会干这事。（可以通过-XX:UseG1GC来启用它。）

###### GC停止时长优化思路
- 堆内存越大，FULL GC需要时间越长。如果应用不能接受长时间的停顿就需要对GC进行优化。
  1. 将FULL GC频率控制在可接收频率，通过在系统空闲时间定时主动调用GC来确保服务的稳定。
  2. 部署多个使用小内存的应用集群来保证FULL GC的停顿时间不会过长。

##### 内存管理小技巧

- 尽量使用直接量，eg：String javaStr = “ ”;
- 使用StringBuilder和StringBuffer进行字符串连接等操作;（事实上编译器会自动将a+b+c类似字符串连接操作自动优化为StringBuilder(以前是StringBuffer)拼接操作）
- 尽早释放无用对象;
- 尽量少使用静态变量;
- 缓存常用的对象:可以使用开源的开源缓存实现，eg：OSCache，Ehcache;
- 尽量不使用finalize()方法;
- 在必要的时候可以考虑使用软引用SoftReference;