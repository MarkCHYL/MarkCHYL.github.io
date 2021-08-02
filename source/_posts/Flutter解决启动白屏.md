---
layout: post
title: Flutter解决启动白屏
toc: true
reward: true
date: 2019-11-14 09:36:51
tags: [Flutter]
categories: [Flutter]
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
```
import org.devio.flutter.splashscreen.SplashScreen;


public class MainActivity extends FlutterActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    SplashScreen.show(this, true);// here
    super.onCreate(savedInstanceState);
    GeneratedPluginRegistrant.registerWith(this);

    ///注册自己的 Plugin 插件
    resisterSelfPlugin();
  }
```
至于额为什么字体显示红色，别紧张，运行没得问题的。
其他的配置看官方文档。
添加
`launch_screen.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"
        android:src="@mipmap/screen_full" />
</RelativeLayout>
```

**下面是我 IOS 配置：**
* 导入启动图片
![](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/WechatIMG309.png?raw=true)
* 配置启动图片
![](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/WechatIMG310.png?raw=true)




