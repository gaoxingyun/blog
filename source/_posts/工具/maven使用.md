---
title: maven使用
date: 2017-06-23 09:12:31
categories: 
- 工具
tags:
- 工具
- java
---


####

- 添加jar包到maven本地仓库
```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=9.0.2.0.0 -Dpackaging=jar -Dfile=ojdbc14.jar
```
