---
title: java异步编程总结
date: 2018-01-09 09:25:54
categories: 
- 总结
tags:
- 总结
- 异步
- java
---


#

##

###### 常用库

- [rxJava](https://github.com/ReactiveX/RxJava) 安卓上流行
- [projectreactor](https://github.com/reactor/reactor-core) spring默认实现
- [vert.x](http://vertx.io/)



###### 名词

- Callback Hell 回调地狱
- 背压 背压是指在异步场景中，被观察者发送事件速度远快于观察者的处理速度的情况下，一种告诉上游的被观察者降低发送速度的策略。简而言之，背压是流速控制的一种策略。
