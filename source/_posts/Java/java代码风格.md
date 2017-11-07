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
