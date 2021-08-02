---
layout: post
title: Android APK反编译 apktool使用教程
toc: true
reward: true
date: 2019-09-26 09:48:57
tags: [工具使用,反编译]
categories: [Android]
---
## 前言：

时间久了之前在熟悉的东西也会渐疏，好记性不如乱笔头。

## 工具介绍

- apktool  

​     作用：主要查看res文件下xml文件、AndroidManifest.xml和图片。（注意：如果直接解压.apk文件，xml文件打开全部是乱码）

- dex2jar

​     作用：将apk反编译成[Java](http://lib.csdn.net/base/javaee)源码（classes.dex转化成jar文件）

- jd-gui

​     作用：查看APK中classes.dex转化成出的jar文件，即源码文件
<!--more-->

## 新版本apktool用法：

1、下载和安装方法：

https://ibotpeaches.github.io/Apktool/install/

![](https://img-blog.csdnimg.cn/20200326094455532.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpcmtzbWFsbGVy,size_16,color_FFFFFF,t_70)

**需要注意的是**
步骤一种的脚步，另存为apktool后，一定要把下载的apktool_xxxx.jar重命名为apktool.jar ，并且要和apktool脚本放在同一级目录下。

2、使用方法

https://ibotpeaches.github.io/Apktool/documentation/

`apktool d -f xxxx.apk`

3、反编译开始

反编译：

```apktool d test.apk -o test```

回编译

```apktool b test -o new_test.apk```



## 蚂蚁虽小也是肉，细心积累，慢慢提高自己，加油！
