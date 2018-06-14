---
title: 使用github搭建wiki知识库
date: 2017-07-16 12:06:19
categories: 
- 工具
tags:
- 工具
---


#### 

```bash

# 查看目录
$ ls -F | grep '/$' | awk -F '/' '{printf $1"\n"}'

# 目录拼接链接
$ ls -F | grep '/$' | awk -F '/' '{printf $1"\n"}' |  awk -F '.' '{printf "- ["$1"]("$1")""\n"}'

# 拼接链接
$ ls -l | grep -v total | awk '{printf $9"\n"}' | awk -F '.' '{printf "- ["$1"]("$1")""\n"}'

```