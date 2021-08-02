---
layout: post
title: android反编译apktool---dex2jar---jdgui
toc: true
reward: true
date: 2020-12-14 13:46:59
tags: [工具搭建,反编译]
categories: [Android]
---

## 反编译的工具（MAC环境下）
下载地址：
  * [apkTool：一款用于反编译apk资源文件](https://ibotpeaches.github.io/Apktool/install/)
  * [dex2jar：是用于将class.dex 转换成classes-dex2jar.jar的工具](https://sourceforge.net/projects/dex2jar/files/)
  * [jdgui：这个用于查看classes-dex2jar.jar 源码工具](http://java-decompiler.github.io/)
  
  <!-- more -->

## apktool下载安装使用
1、下载

![](https://img-blog.csdnimg.cn/20191226175801984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)

 * 将你下载的apkttool工具放到/usr/local/bin
  ![](https://img-blog.csdnimg.cn/20191226185054248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)
 * 指向对应的文件夹cd /usr/local/bin
  ![](https://img-blog.csdnimg.cn/20191226185223643.png)
 * 给apktool权限：输入以下两个命令行给予这两个文件夹权限
    * chmod a+x apktool.jar
    * chmod a+x apktool
 * 验证是否成功
  终端输入`apktool -version`
  
2、使用反编译命令行 apktool d xxxx.apk
反编译之后的文件夹中的文件介绍
![](https://img-blog.csdnimg.cn/20191226190211334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20191226190234935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)
假如只需要获取app的xml或者图片资源，则到这一步则完成啦！


如果要获取到java的代码，那么我们还要进行回编译，这样，我们就能获取到classex.dex文件了

在终端执行命令 `apktool b app反编译的文件夹名称` ，执行完毕后，我们就可以在刚刚的文件夹里面多了一个` build `文件夹了，里面的 `classess.dex `则是我们想要获取到的源码文件

## dex2jar操作流程
下载
![](https://img-blog.csdnimg.cn/20191226190424255.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)
下载完成解压文件后，再终端执行 `chmod +x d2j-dex2jar.sh `和 `chmod +x d2j_invoke.sh` 添加运行权限：
 * 拿出里面的`classe.dex`，放入到`dex2jar`文件夹中
 * 使用命令行 `sh d2j-dex2jar.sh classes.dex`,就可以转换成`classes-dex2jar.jar `
![](https://img-blog.csdnimg.cn/20191226190907884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YWlndWFpXzIwMTU=,size_16,color_FFFFFF,t_70)

## jd-gui反编译class文件查看java代码
下载文件放入自己的安装目录，解压即可使用
然后把刚刚我们获取到的`classes-dex2jar.jar`拖进去打开文件即可。