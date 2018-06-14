---
title: http协议基础知识
date: 2017-06-19 10:58:02
categories: 
- 基础
tags:
- 基础
- 协议
---

#### http状态码

- 200 成果
- 302 重定向
- 404 找不到资源
- 5xx 服务器错误


#### http请求头

##### Transfer-encoding

- Transfer-encoding是一个 HTTP 头部字段，字面意思是「传输编码」。
- Transfer-Encoding: chunked 就代表这个报文采用了分块编码。这时，报文中的实体需要改为用一系列分块来传输。每个分块包含十六进制的长度值和数据，长度值独占一行，长度不包括它结尾的 CRLF（\r\n），也不包括分块数据结尾的 CRLF。最后一个分块长度值必须为 0，对应的分块数据没有内容，表示实体结束。

##### Connection

- Connection: keep-alive

##### Content-Disposition

- Content-Disposition指示了下载文件的默认名称

## 问题

- 问题：http地址本地访问正常，但是经过数据中心的转发就显示空白页面。
- 答案：经查，http协议使用1.1，有Transfer-Encoding: chunked 表示分块传输，响应报文分两次传输，但是转发有限制，只能接收一次响应，因此http响应报文丢失了报文体，因此显示空白。后来数据中心使用了nginx做反向代理，可以正常使用。

