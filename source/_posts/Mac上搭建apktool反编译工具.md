---
layout: post
title: Mac上搭建apktool反编译工具
date: 2019-06-13 15:32:38
toc: true
reward: true
tags: [工具搭建,反编译]
categories: [工具使用]
---
apktool下载地址：
`https://ibotpeaches.github.io/Apktool/install/`
<!--more-->
### 1.在Chrome中，打开网址，找到图中所示Mac相关部分
![](https://img-blog.csdnimg.cn/20191226175801984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)
### 2.在Chrome中，打开网址，在“ wrapper script”上点击鼠标右键，选择“链接存储为”
![](https://upload-images.jianshu.io/upload_images/13001414-fb0faffd01c74a08.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)
存储时，必须选择格式为“所有文件”，文件名为“apktool”，没有后缀(貌似有些浏览器不能选择格式，我在Chrome浏览器可以选择文件格式)
 ![](https://upload-images.jianshu.io/upload_images/13001414-217bcc315f02b99f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)
### 3.点击“find newest here”进入下载页面，选择合适的版本，下载.jar文件，下载后文件名要改为“apktool.jar”
 ![](https://upload-images.jianshu.io/upload_images/13001414-16b2c8e8c0b1dc31.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)
 ![](https://upload-images.jianshu.io/upload_images/13001414-ae3dbce397cce0b3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

### 4.把apktool和apktool.jar文件移动到"/usr/local/bin"目录下，并使用终端命令为两个文件增加执行权限
命令如下：
`chmod +x apktool.jar apktool`
### 5.验证是否安装成功
在终端输入命令：apktool
打印如下信息：

!["安装成功的打印新信息"](https://img-blog.csdnimg.cn/20191226185747298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70 '安装成功的打印新信息')