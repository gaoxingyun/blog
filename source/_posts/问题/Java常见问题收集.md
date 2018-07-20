---
title: java常见问题收集
date: 2017-03-03 17:02:28
categories: 
- 问题
tags:
- 问题
---

## Java常见问题收集

1. String类为什么是final的。
  - 安全性，效率
2. HashMap的源码，实现原理，底层结构。
  - 数组 + 链表
  - 初始因子为16，满了后扩容。通过计算key的hash值取模得到数据在数组中的下标，直接访问。
3. Java有内存泄漏吗？
  - 语法级别没有，实际中文件等资源不关闭可能会导致资源一直占有，类似于内存泄漏。
4. 一个java文件可以有几个类？
  - 最多只能有一个public类，且此类名与java文件名相同，如没有public类，文件名可以任意和内部类名相同，一个文件多个类会在编译时会生成（文件名$数字）名称的class文件，每个数字代表一个类。
5. Collection和Collections的区别？ 
  - Collection是一个接口，它是Set、List等容器的父接口；Collections是个一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包括对容器的搜索、排序、线程安全化等等。
6. TreeMap和TreeSet在排序时如何比较元素？Collections工具类中的sort()方法如何比较元素？ 
  - TreeSet要求存放的对象所属的类必须实现Comparable接口，该接口提供了比较元素的compareTo()方法，当插入元素时会回调该方法比较元素的大小。TreeMap要求存放的键值对映射的键必须实现Comparable接口从而根据键对元素进行排序。Collections工具类的sort方法有两种重载的形式，第一种要求传入的待排序容器中存放的对象比较实现Comparable接口以实现元素的比较；第二种不强制性的要求容器中的元素必须可比较，但是要求传入第二个参数，参数是Comparator接口的子类型（需要重写compare方法实现元素的比较），相当于一个临时定义的排序规则，其实就是通过接口注入比较元素大小的算法，也是对回调模式的应用（Java中对函数式编程的支持）。
7. 简述synchronized 和java.util.concurrent.locks.Lock的异同？ 
  - Lock是Java 5以后引入的新的API，和关键字synchronized相比主要相同点：Lock 能完成synchronized所实现的所有功能；主要不同点：Lock有synchronized更精确的线程语义和更好的性能，而且不强制性的要求一定要获得锁。synchronized会自动释放锁，而Lock一定要求程序员手工释放，并且最好在finally 块中释放（这是释放外部资源的最好的地方）。
8. Java中有几种类型的流？ 
  - 字节流和字符流。字节流继承于InputStream、OutputStream，字符流继承于Reader、Writer。在java.io 包中还有许多其他的流，主要是为了提高性能和使用方便。关于Java的I/O需要注意的有两点：一是两种对称性（输入和输出的对称性，字节和字符的对称性）；二是两种设计模式（适配器模式和装潢模式）。另外Java中的流不同于C#的是它只有一个维度一个方向。
9. fail-fast与fail-safe有什么区别？
- Iterator的fail-fast属性与当前的集合共同起作用，因此它不会受到集合中任何改动的影响。Java.util包中的所有集合类都被设计为fail->fast的，而java.util.concurrent中的集合类都为fail-safe的。当检测到正在遍历的集合的结构被改变时，Fail-fast迭代器抛出ConcurrentModificationException，而fail-safe迭代器从不抛出ConcurrentModificationException。
10. HashMap与HashTable的区别是什么?
 1. HashTable基于Dictionary类，而HashMap是基于AbstractMap。Dictionary是任何可将键映射到相应值的类的抽象父类，而AbstractMap是基于Map接口的实现，它以最大限度地减少实现此接口所需的工作。（在Java 8中我查看源码发现Hashtable并没有继承Dictionary,而且里面也没有同步方法，是不是java 8中Hashtable不在同步的了？有没有人解释一下？）
 2. HashMap的key和value都允许为null，而Hashtable的key和value都不允许为null。HashMap遇到key为null的时候，调用putForNullKey方法进行处理，而对value没有处理；Hashtable遇到null，直接返回NullPointerException。
 3. Hashtable是同步的，而HashMap是非同步的，但是我们也可以通过Collections.synchronizedMap(hashMap),使其实现同步。
11. Executor和Executors区别？
- Executor是线程池的接口，Executors是创建线程池的类，提供了若干个静态方法，用于生成不同类型的线程池。
12. Collection和Collections区别？
- Collection是集合类的接口，Collections是集合类的工具类，提供了集合的一些装饰方法，比如将线程不安全的集合类转换为线程安全的集合类。
13. 如何线程安全的使用Map？
- ConcurrentHashMap, Hashtable, Collections.synchronizedMap(), ConcurrentHashMap在线程安全的基础上提供了更好的写并发能力，但同时降低了对读一致性的要求。[ConcurrentHashMap解释](http://www.importnew.com/22007.html)
14. 双重检查锁的单例模式为什么在JDK1.5之前是线程不安全的？
- 因为知道JDK1.5才正确的处理了指令重排序的问题，之前由于指令重排序导致线程不安全。
15. SimpleDateFormat为什么是线程不安全的？
- 其内部实现拥有可变状态
16. Java查询数据库拼接sql时常加上条件where 1=1，有什么作用？
- 加上where 1=1当语句没有任何条件时不会报错，否则没有任何条件时语句将变成类似 select * from table where; 这种语句，加where 1=1 主要为了简化拼sql操作，不用另外去判断是否需要where关键字了。
17. Runable和Callable的区别？
- 两者都是接口，都能创建一个线程，但Callable可用通过Feture获得线程的返回值。在需要获得线程执行结果的场景中是呀Callable。
18. Java中对象转换为String的常用方法?
- 方法1、String objStr = (String) obj：    强制类型转换，对象obj为null，结果也为null，但是obj必须保证其本质是String类型的值，即可转换的值。例如，不能强制转换 (String) 123
方法2、String objStr = obj.toString方法    ：调用对象的toString方法，必须保证本类或者父类已经重写了Object类的toString方法，如果没有重写toString方法，将默认调用Object类的toString方法，返回getClass().getName() + '@' + Integer.toHexString(hashCode())，并不是obj的实际字符串表示，同时还必须保证对象obj不能为null，否者调用toString方法会报空指针异常java.lang.NullPointerException。
方法3、String objStr = String.valueOf(obj) :    对象obj为null，转换结果为字符串"null"，否则，返回 obj.toString() 的值。注意，如果为obj为null，这里转换后的值已经是字符串的“null”，判空不能再用 obj == nulll，而应该用 str.equals("null")。
已经知道obj为String类型的情况下，可以直接使用方法1转换为String，转换为String后判null条件为：if (objStr != null)
慎用方法2
对于不知道具体类型的情况下，可以使用方法3，只是转换后String的判null条件改为：if (!objStr.equals('null'))