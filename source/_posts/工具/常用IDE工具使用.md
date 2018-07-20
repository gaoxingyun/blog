---
title: 常用IDE工具使用
date: 2017-02-25 12:11:17
categories: 
- 工具
tags:
- 工具
---

## 常用IDE工具使用

### 安装

#### 插件

###### 阿里编码规范

- [https://github.com/alibaba/p3c](https://github.com/alibaba/p3c)

######  Lombok

- [Project Lombok](https://projectlombok.org)

###### 测试覆盖率
- EclEmma 测试覆盖率插件，基于JaCoCo。


### eclipse使用

#### 调试

##### 快捷键

- F5
- F6
- F7

##### debug模式视图

- 线程堆栈视图
- 表达式、断点、变量视图
- 代码视图

##### 调试功能
###### 多线程调试

- debug模式下，debug下的Suspended线程可以分别选中进行调试，Runing状态线程代表运行中的线程。

###### 后退执行

###### 允许到某行

- debug模式下，右键选择运行到某行（Run to Line）（快捷键：command + U）

###### 条件断点

###### 异常断点

###### 片段代码
Inspect

###### 监控变量或表达式

- 右键watch，进入Expression界面，可添加需监控的变量或表达式值。

###### 变量值修改

- debug模式，Variables下右键选择Change Value可修改变量值，值支持new Object()类似赋值。

##### 远程调试

###### javaSE程序远程调试

1. 在远程操作系统上启动java进程时增加特殊的-Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,address=$DEBUG_PORT,server=y,suspend=n
2. 在Eclipse中新建一个Remote Java Application
3. 远程debug，打开Debug Configurations视图，右击Remote Java Application，New
4. 打开Debug Configurations视图
5. 输入远程IP和端口，端口即服务端的$DEBUG_PORT,点击OK

###### Tomcat远程调试

- 使用JPDA服务
  1. ./catalina.sh jpda start  启动jpda
  2. 更改catalina.sh配置，更改默认调试端口（8000） CATALINA_OPTS="-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5888" 


#### 部署

##### tomcat部署

###### 部署常见问题

- eclispe启动tomcat后提示ClassNotFound，进入tomcat目录发现无class文件  解决：Properties -> Deployment Assembly -> 添加部署路径（src WEB-INF/classes）   需配置，不配置情况下可能eclipse不能自动部署class文件到tomcat

### idea使用

#### idea激活

- 使用docker部署idea license server
- 使用idea license server 激活idea
```
http://idea.ustar.pub
```


#### 快捷键

##### 快捷键修改

- 自动补全快捷键
进入 Preferences -> KeyMap -> Main menu –> Code –> Completion -> Basic，改为自己想要的快捷键即可。

###### 插件

- GsonFormat JSON串转对象
- [google-java-format](https://github.com/google/google-java-format)




###### 问题
- [Intellij IDEA 使用Spring-boot-devTools无效解决办法](http://blog.csdn.net/wjc475869/article/details/52442484)
- idea控制台中文乱码   vm参数添加 -Dfile.encoding=UTF-8   Windows系统使用 -Dfile.encoding=GBK
- idea导入gradle项目出错`Error:Failed to open zip file. Gradle's dependency cache may be corrupt`,修改gradle-wrapper.properties里的gradle的版本[http://blog.csdn.net/qq_31001287/article/details/54629570](http://blog.csdn.net/qq_31001287/article/details/54629570)

### Atom工具使用

#### 插件

- 代码跳转 hyperclick + goto-definition

