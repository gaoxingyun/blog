---
title: jdkApi设计
date: 2017-03-22 22:40:00
categories: 
- Java
tags: 
- java
---

# jdkApi设计

##### ThreadLocal设计
- 最初设计是在ThreadLocal对象中维护一个Map，以线程号为key，实现多线程属性的隔离。后面改为在Thread中维护Map，以ThreadLocal对象为键。此种实现的优点：
 1. 线程销毁后，维护的Map自动释放（GC）。
 2. Map的大小变为ThreadLocal对象的个数，高并发情况下Map大小明显减少，提高了效率。
 3. 实现了通过ThreadLocal隔离属性的完全隔离。

- get代码
```java
   public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }
```
- getMap(t)代码
```java
ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }
```

- Thread代码
```java
ThreadLocal.ThreadLocalMap threadLocals = null;
```
