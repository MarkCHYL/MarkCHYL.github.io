---
layout: post
title: mac下安装rz sz
toc: true
reward: true
date: 2021-09-17 15:57:47
tags: [工具]
categories: [Linux]
---
我们在linux上部署代码的时候经常需要上传文件到linux，有时候也需要从linux上下载文件到本地，大部分人都直接借助于ftp工具，
然而其实我们可以直接通过rz和sz上传下载文件，但是rz和sz命令不是linux默认自带的命令，需要我们自己安装，那么如何安装呢

### 手动安装
* 下载lrzsz安装包
`wget http://www.ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz `
* 解压并切换到lrzsz-0.12.20目录下面
`tar zxvf lrzsz-0.12.20.tar.gz && cd lrzsz-0.12.20`
* 编译
  切换到文件解压目录下，在终端中执行如下命令编译
  `./configure && make && make install`
* 上面安装过程默认把lsz和lrz安装到了/usr/local/bin/目录下，现在我们并不能直接使用，下面创建软链接，并命名为rz/sz：
    ```
    cd /usr/bin
    ln -s /usr/local/bin/lrz rz
    ln -s /usr/local/bin/lsz sz
    ```
### yum命令安装
    yum -y install lrzsz

### 使用：
    sz -y 下载
    rz -y 上传