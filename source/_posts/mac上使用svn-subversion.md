---
layout: post
title: mac上使用svn(subversion)
toc: true
reward: true
date: 2021-10-09 10:21:15
tags: [工具使用]
categories: [工具使用]
---
[原文：Wanna_1314](https://www.jianshu.com/p/79116c6f8f72)
### 简介
在Windows中，我们常用TortoiseSVN这个软件来进行搭建SVN环境，那么在macOS中，我们应当如何去搭建SVN环境呢？在以前的老版本的macOS中，macOS中Xcode已经为我们提供了SVN的服务端和客户端。但是在如今的新版本当中，已经没有SVN了，我们可以使用Homebrew进行安装SVN。

### 1.安装
如何安装呢？我们使用Homebrew进行安装，关于Homebrew的安装，可以从参考我往期的文章，下面直接讲述安装之后的安装命令
```
brew install subversion
```
我们使用如下的命令进行检查是否安装了这个软件
```
brew list
```
得到如下的结果
![](https://upload-images.jianshu.io/upload_images/24412352-77bcc086d7bbdce9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1170/format/webp)
使用
```
svn help
```
命令查看svn是否可以全局访问，如图所示，则安装成功！
![](https://upload-images.jianshu.io/upload_images/24412352-e47e3ebfa90a4882.png?imageMogr2/auto-orient/strip|imageView2/2/w/1158/format/webp)

### 2.安装之后的配置
使用如下命令创建一个SVN的代码仓库（目录改成你想要创建的目录）
```
svnadmin create /Users/wanna/Desktop/Code/SVN
```
**当然我的电脑上自动给我创建了**
`/Users/mark/.subversion`
以下不多赘述，结合原文查看。