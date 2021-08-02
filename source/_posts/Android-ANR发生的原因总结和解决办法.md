---
layout: post
title: Android ANR发生的原因总结和解决办法
toc: true
reward: true
date: 2020-08-06 10:10:16
tags: [Error,基础理论]
categories: [Android]
---
### 什么是ANR？
> ANR的全称是application not responding，是指应用程序未响应，Android系统对于一些事件需要在一定的时间范围内完成，如果超过预定时间能未能得到有效响应或者响应时间过长，都会造成ANR。一般地，这时往往会弹出一个提示框，告知用户当前xxx未响应，用户可选择继续等待或者Force Close。
***
### 官方指定的产生ANR一般有以下类型：
1：KeyDispatchTimeout（5秒）–主要类型
按键或触摸事件在特定时间无响应

2：BroadcastTimeout（10秒）
BroadcastReceiver在特定时间无法处理完成

3：ServiceTimeout（ 20秒）–小概率类型
服务在特定的时间无法处理完成

***
### 导致ANR的根本原因

1.主线程执行了耗时操作，比如数据库操作或网络编程,I/O操作

2.其他进程（就是其他程序）占用CPU导致本进程得不到CPU时间片，比如其他进程的频繁读写操作可能会导致这个问题。

细分的话，导致ANR的原因有如下几点：
   1.耗时的网络访问
   2.大量的数据读写
   3.数据库操作
   4.硬件操作（比如camera)
   5.调用thread的join()方法、sleep()方法、wait()方法或者等待线程锁的时候
   6.service binder的数量达到上限
   7.system server中发生WatchDog ANR
   8.service忙导致超时无响应
   9.其他线程持有锁，导致主线程等待超时
   10.其它线程终止或崩溃导致主线程一直等待

***
### 调查并解决ANR
* 1、首先分析log
* 2、从trace.txt文件查看调用堆栈。-$ adb拉取data / anr / traces.txt。
* 3、看代码
* 4、仔细查看ANR的成因（iowait？block？memoryleak？）
***
### 如何避免ANR的发生
1.避免在主线程执行耗时操作，所有耗时操作应新开一个子线程完成，然后再在主线程更新UI。

2.BroadcastReceiver要执行耗时操作时应启动一个service，将耗时操作交给service来完成。

3.避免在Intent Receiver里启动一个Activity，因为它会创建一个新的画面，并从当前用户正在运行的程序上抢夺焦点。如果你的应用程序在响应Intent广 播时需要向用户展示什么，你应该使用Notification Manager来实现。
