---
title: 使用gitpages搭建静态博客
date: 2017-07-16 12:06:19
categories: 
- 工具
tags:
- 工具
- 博客
---

## 静态博客生成工具

#### 使用jekyll工具生成静态博客
- 

#### 使用hexo工具生成静态博客

###### 参考文档
[https://pages.github.com](https://pages.github.com)
[https://hexo.io](https://hexo.io)
[http://theme-next.iissnan.com](http://theme-next.iissnan.com)


#### 使用Travis CI在线持续集成工具实现博客自动部署

- Travis CI时一个在线持续集成工具，通过他我们可以实现push代码到远程仓库后，自动发布部署我们的博客。

###### 参考文档
[https://segmentfault.com/a/1190000004667156](https://segmentfault.com/a/1190000004667156)


#### 为静态博客添加自定义域名

1. 购买域名
2. 添加CNAME解析记录，例如：
```
CANME       @       blog.ustar.pub
CNAME       blog    gaoxingyun.github.io
```
3.在github博客仓库顶层添加CNAME文件，并把自定义域名地址填入CNAME文件。这里有一个坑，域名地址需换行，否则会依然显示404页面，例如：
```
blog.ustar.pub

```
另外，CNAME记录转发到gitpages的服务器上，服务器决定展示到哪个博客是由github博客的CNAME文件中的地址和HTTP请求的HOST决定的，因此不能两个博客的CNAME地址填写同一个域名，这样github无法决定访问哪个博客。同时CNAME仅支持一个域名，因此在CNAME文件中填写多个域名无效。

