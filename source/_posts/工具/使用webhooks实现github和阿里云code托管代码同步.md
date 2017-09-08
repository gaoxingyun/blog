---
title: 使用webhooks实现github和阿里云code托管代码同步
date: 2017-07-16 20:14:32
categories: 
- 工具
tags:
- 工具
- github
---

##
- github的私有仓库太贵，自己搭建的gitlab又太费服务器资源，好在阿里云提供了一个代码托管平台[https://code.aliyun.com](https://code.aliyun.com)，可以创建私有仓库。

1. 将github代码导入到阿里云code。
2. 为github仓库创建webhooks事件。

## webhooks推送

- github webhooks文档
[https://developer.github.com/webhooks/](https://developer.github.com/webhooks/)
