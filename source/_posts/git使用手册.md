---
title: git使用手册
date: 2016-12-07 13:44:22
categories: 
- 总结
tags: 
- 工具
---

## 命令

#### git add
> git add + 文件／路径 添加文件到本地暂存区

#### git branch
> git branch -d + 分支名 删除分支
> git branch + 分支名 创建分支

#### git checkout
> git checkout + 分支名 切换到指定分支
> git checkout -b + 分支名 创建并切换到指定分支

#### git clone
> git clone + 仓库地址 克隆远程仓库代码到本地

#### git commit
> git commit -m 提交标签

#### git help
> git help + 命令名 查看命令详细帮助

#### git status
> 查看状态

## 分支模型
#### 常用分支
- 主分支 master
- 开发分支 dev
#### 分支使用
- 主干开发，分支发布
- 分支开发，主干发布



## 最佳实践
#### 使用场景
* 在远程仓库新建一个项目，并克隆到本地
``` shell
git clone git@172.16.8.158:gao.xy/xyWeb.git;  
cd xyWeb;
touch README.md;
git add README.md;
git commit -m add README;
git push -u origin master; 
```

* 将本地项目已有项目上传到远程仓库
```
cd existing_folder;
git init;
git remote add origin + git仓库地址;
git add .;
git commit -m “init";
git push -u origin master;
```

* 新建开发分支进行开发
```
git branch dev;
git checkout dev;
```

* 合并开发分支到主分支
```
git chekout master;
git merge dev;
```


