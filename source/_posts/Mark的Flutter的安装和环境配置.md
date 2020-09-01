---
layout: post
title: Mark的Flutter的安装和环境配置
date: 2018-12-17 11:08:44
toc: true
reward: true
tags: [Android,Flutter]
categories: [Flutter]
---
## 前言：
        我是一名Android原生开发程序员，目前熟练掌握Java语言和kotlin语言去编写安卓。RN出来的时候我没去学习，现在我想赶上Flutter这列火车。
<!--more-->
* * *

   ### 我的配置：

*    Mac笔记本
*    Android Studio 3.2.0 版本
*    准备了翻墙的软件，貌似没怎么用上

* * *


### 第一步：获取Flutter代码

1. 我先选择了一个存储Flutter的文件夹：

    ![56a74cdc0cf7522961022dcd31a0f3ca.png](evernotecid://3DE7A385-FFCA-4630-91A9-F88E73A63AF0/appyinxiangcom/22109192/ENResource/p1)
名字跟随自己的命名习惯。
2. 克隆Flutter代码到本地目录：
两种方式：自己在GitHub 上面把SDK下下来放到文件中，或者通过终端输入：
```
git clone  https://github.com/flutter/flutter.git
```
    最后就是一样的配置：open .bash_profile打开添加以下路径：
```
export PATH=本地目录/flutter/bin:$PATH
```
        配置完了后，应该可以运行Flutter命令了！可以使用下面命令flutter doctor来检查依赖环境是否正常。如果下载有问题的话，可能是网络受限，如果你没有翻墙软件的话，在终端输入如下两条命令：
```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
官方也给出了方法，就是使用国内的镜像节点下载，在运行flutter doctor之前先运行下面两个命令即可。
![a51a64a974501d1bf53d7f4cd5f82ab2.png](evernotecid://3DE7A385-FFCA-4630-91A9-F88E73A63AF0/appyinxiangcom/22109192/ENResource/p2)

* * *
### 第二步：配置IOS环境
我在 App Store下载安装好了Xcode，然后自己安装了Homebrew：
[Homebrew配置方法地址：](https://brew.sh/)
接下来我就是执行如下的命令：
open -a Simulator -- 测试成功打开模拟器

###### 安装用于将Flutter应用部署到iOS设备的工具
$ brew update
$ brew install --HEAD libimobiledevice
$ brew install ideviceinstaller ios-deploy cocoapods
$ pod setup
此处我就是无耻的copy的别人的文章，那位大佬的文章地址：[Flutter轻松入门（一） -- MAC环境搭建](https://www.jianshu.com/p/0b73cc2af65f)
* * *
### 第三步：在Android Studio上配置Android开发环境
插件怎么安装就不废话啦，直接截图，它会自动安装Flutter和Dart语言插件，Dart有时候可能会安装失败，请耐心点，也可以单独搜Dart进行安装。
![cd5198371c3b49034b3de9a8c41ecf19.png](evernotecid://3DE7A385-FFCA-4630-91A9-F88E73A63AF0/appyinxiangcom/22109192/ENResource/p3)
### 最后终端运行 Flutter Doctor
检查Flutter开发环境是否配置完成，我的如下图：
![586b6df21a8997dcfeb10164a1e39ec4.png](evernotecid://3DE7A385-FFCA-4630-91A9-F88E73A63AF0/appyinxiangcom/22109192/ENResource/p4)
* * *
笔记收尾处我还得为微我参考的文章打个广告：[Flutter轻松入门（一） -- MAC环境搭建](https://www.jianshu.com/p/0b73cc2af65f)
