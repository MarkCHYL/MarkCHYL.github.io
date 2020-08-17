---
layout: post
title: Git使用教程之本地仓库的基本操
toc: true
reward: true
date: 2020-08-17 15:43:07
tags: [工具]
---
Git 的安装就不再次啰嗦了：
`sudo apt-get install git`
### 1.创建代码仓库
#### Step 1：先配置下身份，这样在提交代码的时候Git就可以知道是谁提交的，命令如下：
```
git config --global user.name "MarkCHYL"
git config --global user.email "2285581945@qq.com"
```
检查下配置是否成功：
```
mark@Markxiansheng blog % git config --global user.name 
MarkCHYL
mark@Markxiansheng blog % git config --global user.email
2285581945@qq.com
```
#### Step 2：找个地方创建我们的代码仓库
>git init

继续输入：ls - al可以看到下目录下有个.git的文件夹就是他了

### 2.提交本地代码
创建完代码仓库，接下来说下如何提交代码.先用add命令把要提交的内容都加进来，然后commit才是真的去执行提交操作!

> git add readme.md
> 
> git commit -m "First Commit"

不过如果我们改动的文件很多的话，我们可以`git add .`一次添加全部.

### 3.查看修改内容
使用`git status`可以查看 修改的部分
但是还没有提交，如果我们想看下具体更改了什么，我们可以用到git diff命令，另外，按Q可以退回命令行输入！

### 4.查看提交记录
>git log
```
commit 19fb3091a83d7d5cc463fa9df67964ab95a2a404 (HEAD -> master)
Author: MarkCHYL <2285581945@qq.com>
Date:   Mon Aug 17 15:57:11 2020 +0800

    First Commit
```
依次是：
* 此次提交对应的版本号
* 提交人：姓名 邮箱
* 提交的时间
* 提交版本修改的内容：就是我们commit -m "xxx"里的xxx

### 5.撤销未提交的修改
比如我们刚提交了一个版本，然后又乱七八糟地写了一堆东西，突然发现不小心误删了一些东西，然后ctrl + s保存了，这个时候是不是欲哭无泪，不过有Git，只需一个checkout命令即可撤销更改，当然是你还没add的情况，比如我们在MainActivity里随便添加一条语句，然后ctrl + s保存代码！
> git diff
会得到输出结果
```
diff --git a/readme.md b/readme.md
index e69de29..58940c4 100644
--- a/readme.md
+++ b/readme.md
@@ -0,0 +1 @@
+鄙人日常笔记记录
\ No newline at end of file
```
这里可以看到我们改的内容，我们可以回去把这句代码删掉，但是如果改的有上千行你怎么改， 于是乎这个时候我们可以使用

> git checkout git checkout /Users/mark/Desktop/Document/blog/readme.md

当然，如果我们已经add了的话，那么checkout是没任何作用的，我们要先取消添加才可以撤回提交，使用下述指令：
> git reset HEAD /Users/mark/Desktop/Document/blog/readme.md

>git checkout /Users/mark/Desktop/Document/blog/readme.md

### 6.版本回退
第五点我们教了大家撤销未提交的修改，但加入提交了，我们想回退到之前的某一个版本怎么办? 第四点中我们可以通过git log查看我们的提交记录，我们需要从这里获取一个版本号， 一般我们只需要前七位字符就够了；另外在Git中，用HEAD代表当前版本，上一个版本就是HEAD^， 再上一个版本就是HEAD^^依次类推！我们先Git Log看下版本历史先！

我们回到前一个提交的版本吧，依次键入下述指令：
> git reset --hard HEAD
> 
 >git reset --hard HEAD^
 >
 >git log

 可以看到我们已经回退到了前一个版本了，当然你可以直接这样写：
 > git reset --hard ad2080c

 就是这么简单！回退后，你突然后悔了，想回退回新的那个版本， 可是遗憾的是，你键入git log却发现没有了最新的那个版本号，这怎么办呢... 没事，Git中给你提供了这颗"后悔药"，Git记录着你输入的每一条指令呢！键入：
 >git reflog
 
 你会发现，版本号就在这里：
 然后执行：
 > git reset --hard ad2080c
 