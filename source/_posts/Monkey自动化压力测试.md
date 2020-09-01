---
layout: post
title: Monkey自动化压力测试
toc: true
reward: true
date: 2020-06-22 10:53:50
tags: [Android]
categories: [Android]
---
## 第一部分 背景
 ### 1、为什么要开展压力测试？
  * 提高产品的稳定性
  * 提高产品的留存率
  ### 2、什么时候开展压力测试？
  * 首轮功能测试通过后开始
  * 下班后的夜间进行
## 第二部分 场景
### 手工测试场景
### 自动化测试场景
1、 Monkey
Monkey 就在手机里，安卓系统再带的

2、 什么事ADB？
Android Debug Bridge 安卓调试桥
<!-- more -->
3、 MonkeyScript
MonkeyScript是一组可被monkey识别的命令集合。
MonkeyScript可以完成重复固定的指令

4、 MonkeyRunner
提供一系列的API，可以模拟事件及截图操作。

5、Monkey和MonkeyRunner的区别？
Monkey：在adb shell中，生成用户或系统的伪随机事件。
MonkeyRunner：通过API定义特定命令和事件控制设备。

MonkeyRunner APIs
*  MonkeyRunner:用来连接设备或者模拟器
*  MonkeyDevice：提供安装、卸载应用，发送模拟事件
*  MonkeyImage：完成图像保存，及对比操作。

### 压力测试结果
crash和ANR

崩溃和应用无响应

## 第三部分：实践
* 步骤一：打开手机USB调试功能
* 步骤二：确认手机和电脑已经连接，执行`adb devices`
  ![效果图](https://thumbnail0.baidupcs.com/thumbnail/c2439e434kb986d6a3989946387f8455?fid=3761439320-250528-622176958452329&time=1592805600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Nqvywj6pwih9WbcOB8SokjVreIw%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=567530952357307099&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)
* 步骤三：安装测试app，`adb install package.apk`
* 步骤四：发送压测指令 `adb shell monkey 1000`执行1000次随机操作指令
* 步骤五：获取app报名：执行`adb logcat | grep START`
* 步骤六：给指定的包打压力；执行`adb shell monkey -p package 1000`

## 第四部分：Monkey高级参数的应用
### 1、throttle参数
指定事件之间的间隔
`adb shell monkey --throttle <milliseconds>`
### 2、seed参数实践
指定随机生成数的seed值 `adb shell monkey -s <seed> <event-count>`

例如：`adb shell monkey -p com.android.calculator2 -s 100 50`
每次执行结构都是一样，记住清理缓存记录再测试，复现问题
### 3、触摸事件
设定触摸事件百分比
`adb shell monkey --pct-touch <percent>`

例如：`adb shell monkey -v -p com.cares.rcs --pct-touch 100 100`，加上`-v`是为了看清具体执行的操作是什么

### 4、动作事件
设定动作事件百分比`adb shell monkey --pct-motion <percent>`
例如：`adb shell monkey -v -p com.cares.rcs --pct-touch 50 --pct-motion 30 100`
执行touch事件50%，motion事件30%，执行一百次事件

### 5、轨迹球事件
设定轨迹球事件百分比
`adb shell monkey --pct-trackball <percent>`
### 6、基本导航事件
设定基本导航事件百分比，输入设备上的上、下、左、右
`adb shell monkey --pct-nav <percent>`
### 7、主要导航事件
设定主要导航事件百分比，兼容中间键、返回键、菜单按键
`adb shell monkey --pct-majornav <percent>`
### 8、系统导航事件
设定系统导航事件百分比，HOME、BACK、拨号及音量按键
`adb shell monkey --pct-syskeys <percent>`
### 9、启动Activity事件
设定启动Activity的事件百分比
`adb shell monkey --pct-appswitch <percent>`
### 10、不常用事件
`adb shell monkey --pct-anyevent <percent>`
### 11、崩溃事件Crash事件
忽略崩溃和异常
`adb shell monkey --ignore-crashes <percent>`
### 12、超时事件ANR事件
忽略超时事件
`adb shell monkey --ignore-timeouts <event-count>`

## 第五部分：CRASH结果提取

 `adb shell monkey -p com.cares.rcs 1000`不断测试执行随机操作

## 第六部分：ANR查看
```
Markxiansheng:Desktop mark$ adb shell
root@vbox86p:/ #
root@vbox86p:/ # cd /data/anr
root@vbox86p:/data/anr # ls
traces.txt
```
## Monkey Script学习使用
> 执行语句：`adb shell monkey -f <scriptfile> <event-count>`

常用命令：

1、**DispatchTrackball**轨迹球事件

2、**DispatchPointer**点击事件

3、**DispatchString**输入字符串事件
`DispatchString(String text)`

4、**LaunchActivity**启动应用事件
`LaunchAtivity(package,Activity)`

5、**UserWait**命令
等待事件：`UserWait(millinsons)`

6、**DispatchPress**命令
按下键值：`DispatchPress(int keycode)` #keycode 66 回车键》

模拟输入查询一千次
* 1、启动App
* 2、点击输入框
* 3、输入查询词
* 4、点击键盘的回车
* 5、点击搜索按钮
* 6、等待结果的出现
* 7、点击clear按钮
Mark.script脚本
```
typ = user
count = 10
speed = 1.0
start data >>

LaunchActivity(com.android.browser,.BrowserActivity)
UserWait(2000)
DispatchPointer(10,10,0,200,200,1,1,-1,1,1,0,0)
DispatchPointer(10,10,1,200,200,1,1,-1,1,1,0,0)
DispatchString(Mark)
UserWait(1000)
DispatchPress(66)
DispatchPointer(10,10,0,600,300,1,1,-1,1,1,0,0)
DispatchPointer(10,10,1,600,300,1,1,-1,1,1,0,0)
UserWait(3000)
```

小黑技术：SDK目录下tools文件中的bin目录下的 uiautomatorviewer.bat,双击启动，用来获取界面控件位置


## Monkey Runner学习使用
主要是利用python编写脚本文件进行测试，暂停学习