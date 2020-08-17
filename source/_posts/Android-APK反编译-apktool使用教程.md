---
layout: post
title: Android APK反编译 apktool使用教程
toc: true
reward: true
date: 2019-09-26 09:48:57
tags: [工具使用]
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

![](https://thumbnail0.baidupcs.com/thumbnail/38e813d52e3dfc10234ae12835819ff7?fid=3761439320-250528-346821393816278&time=1569459600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Z3HFxH7YdDPEq0mUN8y6UzOnddE%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=460436671360346953&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)

2、使用方法

https://ibotpeaches.github.io/Apktool/documentation/

![](https://thumbnail0.baidupcs.com/thumbnail/b2f82f60dfcffa6927c2d670daa72d38?fid=3761439320-250528-558451696215383&time=1569459600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-CsFMBIHt1kd3a8Va1ABYkxhUg9I%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=460411839476402063&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)

3、反编译开始

Mark获得结果只有res文件的是可读性的：

![](https://thumbnail0.baidupcs.com/thumbnail/f9ced22da6e5c604c33ff6227c86ddb6?fid=3761439320-250528-1034439241785614&time=1569459600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-nRulMxiMtqZORfZtt33jt7cyZPo%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=460498225017852308&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)



## 蚂蚁虽小也是肉，细心积累，慢慢提高自己，加油！
