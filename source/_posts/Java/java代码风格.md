---
title: java代码
date: 2016-12-08 13:41:50
categories: 
- 总结
tags:
- java
---

## 代码格式

## 命名规范

#### 类
- 一个好的名称通常是名词或动词的名词形式
#### 方法
- 方法通常表示动作，因此使用动词
#### 属性
- 属性使用名词


## 约定

对外的参数使用包装类型，内部参数使用基本类型


#### 代码写法

- 不要把方法所有的代码用if-else包括，此类代码可以使用if(!条件){;} 正常逻辑来改写。if语句不要包含大段代码，大段代码写在外部。

```
if(...){
    ...;
}else{
    ...;
}
//  改写为
if(!...){

}
...;
```

- 通过三目运算符过滤null值

```
// 通过三目运算符过滤null值为""值
String str = s==null ? "":s;
```

- 不要命名转瞬即逝的中间变量[Point Free代码风格](http://www.ruanyifeng.com/blog/2017/03/pointfree.html)

``` kotlin
var ret = "test";
return ret;
```




#### 异常

- 方法中的异常统一转换为uncheckd exception
```
// check异常转为uncheck异常
try{
    ;
}catch(Exception e)
{
    throw new RuntimeException(e);
}

```


#### 预编译

- 使用正则表达式时，使用其预编译功能，可以加快正则匹配速度
```
// 将正则表达式设置为静态属性
private final static Pattern pattern = Pattern.compile("([A-Za-z\\d]+)(_)?");

```

## 常用命令

### 包

- model 实体
- entity 实体
- service 服务
- util 工具
- validator 校验


### 接口

- I 接口开头



### 类

- Impl 实现类结尾 


#### 领域模型

- POJP 简单JAVA对象
- VO value object 值对象
- BO business object 业务对象
- PO persistant object 持久对象
- DAO data access object 数据访问对象

- CO Client Object 客户端交互对象 API交互对象
- DO Data Object 数据对象


### 方法

### 变量

### 枚举

### 设计模式

- Singleton 单件
- Abstract Factory 抽象工厂模式
- Builder 生成器模式
- Factory Method 工厂方法模式
- Prototype 原型模式
- Adapter 适配器模式
- Bridge 桥接模式
- Composite 组合模式
- Decorator 装饰模式
- Facade 门面模式
- Flyweight 享元模式
- Proxy 代理模式
- Template Methed 模板方法
- Command 命令模式
- Interpreter 解释器模式
- Mediator 中介者模式
- Iterator 迭代器模式
- Observer 观察者模式
- Chain Of Responsibility 责任链模式
- Memento 备忘录模式
- State 状态模式
- Strategy 策略模式
- Visitor 访问者模式