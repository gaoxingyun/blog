---
title: 使用gitpages搭建静态博客
date: 2017-07-16 12:06:19
categories: 
- 工具
tags:
- 博客
---

## 使用hexo工具生成静态博客

## 为静态博客添加自定义域名

1. 购买域名
2. 添加CNAME解析记录
```
CANME       @       blog.ustar.pub
CNAME       blog    gaoxingyun.github.io
```
3.在博客仓库顶层添加CNAME文件，并把自定义域名地址填入CNAME文件。这里有一个坑，域名地址需换行，否则会依然显示404页面，例如：
```
blog.ustar.pub

```