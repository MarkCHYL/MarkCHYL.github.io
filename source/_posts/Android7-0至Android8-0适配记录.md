---
layout: post
title: Android7.0至Android8.0适配记录
date: 2018-12-17 10:55:42
toc: true
reward: true
tags: [适配]
categories: [Android]
---
## Android7.0至Android8.0适配记录
#### 一五年的老项目，里面存在过时的API众多，导致高版本的编译时运行时报错，而我这次做的适配遇到的困难重要有三个
###    
<!--more-->
* ### 广播接收者（BroadCastReceiver）无法正常己收到广播去发起通知。
* ### 8.0系统的手机上，全面的界面不能填充完整。
* ### 由于项目里用到的网络框架jar是XUtils，许久没做维护的第三方的jar包，HttpClient 和 HttpConnection 已在高版本的API直接和我们拜拜了。

### 下面我真对上面自己的问题记录下我的处理：
##  第一、 8.0系统的手机上，全面的界面不能填充完整
```
     <meta-data
            android:name="android.max_aspect"
            android:value="2.4" />
```
## 第二、 广播接收者（BroadCastReceiver）无法正常己收到广播去发起通知

[关于Android 8.0后notification通知声音无法关闭或开启的问题](https://blog.csdn.net/fzkf9225/article/details/81119780)
## 第三、 HttpClient 和 HttpConnection的强制使用Utils的低版本API

#### 这个我在网上找到一个配置方法，但是还是编译失败，所以弃用，起码现在编译成功

在app中的build文件中添加：
```
  useLibrary 'org.apache.http.legacy'
```
