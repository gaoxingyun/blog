---
title: git使用
date: 2016-12-07 13:44:22
categories: 
- 工具
tags: 
- git
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
- 开发分支 develop
- 特性分支 feature
- 发布分支 release
- 热更新分支 hotfix

#### 工作流

- 集中式工作流
只有一个master分支，开发者将代码克隆到本地，之后所有修改提交都在本地进行，直到某个时间点将本地代码合并到远程master分支。
- 功能开发工作流
关注功能开发，不直接往master提交代码保证它是稳定干净的，而是从master拉取feature分支进行功能开发，团队成员根据分工不同进行不同feature的开发，当功能开发完成后，会向master分支发起pull request，只有通过审核的代码才允许合并入master，也就是Code Review。
- Gitflow工作流
适合管理大型项目的发布和维护，整个开发周期master和develop分支一直存在，特性开发在feature进行，发布在release进行，bug修复在hotfix进行。
- Forking工作流
开源项目常用，有一个公开的中央仓库，其他开发者可以克隆这个仓库作为自己的私有仓库，只有开源项目维护者才可以向中央仓库push代码和接受代码贡献者向中央仓库发起的pull request请求。

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