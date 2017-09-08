---
title: scala语言基础知识
date: 2017-03-03 11:14:33
categories: 
- 基础
tags:
- 基础
- 编程语言
- java
---

# Scala语言

## 语法

#### 变量

##### 数据类型

- Byte	8位有符号补码整数。数值区间为 -128 到 127
- Short	16位有符号补码整数。数值区间为 -32768 到 32767
- Int	32位有符号补码整数。数值区间为 -2147483648 到 2147483647
- Long	64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807
- Float	32位IEEE754单精度浮点数
- Double	64位IEEE754单精度浮点数
- Char	16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF
- String	字符序列
- Boolean	true或false
- Unit	表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。
- Null	null 或空引用
- Nothing	Nothing类型在Scala的类层级的最低端；它是任何其他类型的子类型。
- Any	Any是所有其他类的超类
- AnyRef	AnyRef类是Scala里所有引用类(reference class)的基类

##### 变量声明

- var 变量名: 数据类型 = "";
 eg:
 ```
 var a: String = "a";
 var b: String;
 var c = "c";
 // 以上都是合法的声明，但是变量类型和初始值不可同时省略
 ``` 

#### 访问修饰符

如果没有指定访问修饰符符，默认情况下，Scala对象的访问级别都是 public。

 1. 私有(Private)成员
 2. 保护(Protected)成员
 3. 公共(Public)成员
 4. 作用域保护 Scala中，访问修饰符可以通过使用限定词强调。格式为:private[x]或protected[x],这里的x指代某个所属的包、类或单例对象。如果写成private[x],表示"这个成员除了对[…]中的类或[…]中的包中的类及它们的伴生对像可见外，对其它所有类都是private。这种技巧在横跨了若干包的大型项目中非常有用，它允许你定义一些在你项目的若干子包中可见但对于项目外部的客户却始终不可见的东西。

#### 运算符

##### 运算符分类

Scala 含有丰富的内置运算符，包括以下几种类型：
- 算术运算符 + - * ／ %
- 关系运算符 == != > < >= <=
- 逻辑运算符 && || !
- 位运算符 & | ^ ~ << >> >>>(无符号右移)
- 赋值运算符 = +== -= *= /= %= <<= >>= &= |= ^=

#### 流程控制语句

scala流程控制语句同java一致
- 选择语句
- 循环语句

#### 函数

##### 函数声明

Scala 函数声明格式如下：
def functionName ([参数列表]) : [return type]
如果不写等于号和方法主体，那么方法会被隐式声明为"抽象(abstract)"，包含它的类型于是也是一个抽象类型。

##### 函数定义

方法定义由一个def 关键字开始，紧接着是可选的参数列表，一个冒号"：" 和方法的返回类型，一个等于号"="，最后是方法的主体。
Scala 函数定义格式如下：
def functionName ([参数列表]) : [return type] = {
   function body
   return [expr]
}

#### 闭包

闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。
闭包通常来讲可以简单的认为是可以访问一个函数里面局部变量的另外一个函数。
eg:
```
object Test {  
   def main(args: Array[String]) {  
      println( "muliplier(1) value = " +  multiplier(1) )  
      println( "muliplier(2) value = " +  multiplier(2) )  
   }  
   var factor = 3  
   val multiplier = (i:Int) => i * factor  
}  
```

#### 数组

##### 数组的声明

```
var z:Array[String] = new Array[String](3)或
var z = new Array[String](3)或
var z = Array("Runoob", "Baidu", "Google")
```

##### 多维数组

```
var myMatrix = ofDim[Int](3,3)
```

#### 集合

- Scala List(列表)
List的特征是其元素以线性方式存储，集合中可以存放重复对象。
- Scala Set(集合)
Set是最简单的一种集合。集合中的对象不按特定的方式排序，并且没有重复对象。
- Scala Map(映射)
Map 是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。
- Scala 元组
元组是不同类型的值的集合
- Scala Option
Option[T] 表示有可能包含值的容器，也可能不包含值。
- Scala Iterator（迭代器）
迭代器不是一个容器，更确切的说是逐一访问容器内元素的方法。
