---
layout: post
title: Flutter解决启动白屏
toc: true
reward: true
date: 2019-11-14 09:36:51
tags: [Flutter]
---
### 为什么启动会出现白屏？
由于Android启动的时候要进行一系列初始化，如检查权限，开启进程，绑定application，startActivity。
这些初始化会稍微需要一点点时间，比如一秒钟。白屏持续的时间长短当然也和设备有关，设备越差白屏持续时间越长。
<!--more-->
### 如何解决？
我这里使用第三方的插件进行解决。[flutter_splash_screen](https://pub.flutter-io.cn/packages/flutter_splash_screen).

其使用方法看他的文档。

### 记录下配置的时候遇到的问题？
导包总是报错，先手敲代码，再导包。支持自定义启动布局。
安卓的配置步骤多点。
**下面是我安卓配置：**
![](https://thumbnail0.baidupcs.com/thumbnail/477735a56f90cd1d64169d2917a87770?fid=3761439320-250528-246305797275251&time=1573696800&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-G9FxrL0R8wPZgpghMa0UmLf4liU%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=7361838327784451279&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)
至于额为什么字体显示红色，别紧张，运行没得问题的。
其他的配置看官方文档。
**下面是我 IOS 配置：**
* 导入启动图片
![](https://thumbnail0.baidupcs.com/thumbnail/8f43bb0ad5a59ab03ed661aa656a7046?fid=3761439320-250528-119014613827010&time=1573696800&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-MVtW4kap4%2BVX1pHhbojCnEHjvLc%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=7362027953528330279&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)
* 配置启动图片
![](https://thumbnail0.baidupcs.com/thumbnail/37e039082e477ed6eb598eb35be32040?fid=3761439320-250528-291075177673625&time=1573696800&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Uxe3IMjyZGyTpPolQclxnVFPyAo%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=7362084701809447864&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)


