---
title: gradle使用
date: 2017-02-17 16:34:12
categories: 
- 工具
tags:
- 工具
---

##

#### gradle命令

- gradle build -i 构建，有详细过程日志


####
- compile('org.apache.httpcomponents:httpclient:+') 版本‘+’表示拉取最新版本，但是每次构建都会检查最新版本，会使构建变慢。



#### gradle插件
- [shadow](https://github.com/johnrengelman/shadow)


#### gradle阿里云maven仓库
- maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }