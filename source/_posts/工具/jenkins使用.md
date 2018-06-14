---
title: jenkins使用
date: 2018-04-15 09:39:05
categories:
- 工具
tags:
- 工具
---

## jenkins使用

### 安装

- 通过官网[https://jenkins.io](https://jenkins.io)进行安装


### 使用

###### 问题

- 执行远程shell脚本构建无法停止？由于tomcat等进程启动后不会停止，所有构建无法停止，可以用bash来执行远程构建脚本。例如：`bash -x -s < /home/ci/ci.sh;`

