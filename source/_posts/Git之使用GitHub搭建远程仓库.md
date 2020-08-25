---
layout: post
title: Git之使用GitHub搭建远程仓库
toc: true
reward: true
date: 2020-08-18 16:00:15
tags: [工具]
---
### 1.账号注册&仓库创建：
不做记录，简单
### 2.Clone代码库到本地
<!-- more -->
`git clone https://github.com/ZPJay/Garbage.git`
### 3.分支管理
①创建分支(后者创建同时会切换分支):
> git branch v1.0.3 或 git checkout -b v1.0.4
②查看版本库中所有分支：
> git branch -a
③切换到某一分支：
> git checkout v1.0.3
④删除某一分支：
> git branch -D v1.0.4
⑤合并分支
> git merge v1.0.3

### 4.本地仓库与远程仓库同步问题
先对我们的本地仓库做一点点修改，接着git add和git commit本地准备后，然后：
> git push origin master 或者直接 git push

有同步到服务器，肯定有服务器同步到本地是吧，很简单，就一个
> git pull