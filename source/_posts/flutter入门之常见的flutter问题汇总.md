---
layout: post
title: flutter入门之常见的flutter问题汇总
date: 2019-07-11 15:25:43
toc: true
reward: true
tags: [Flutter,Flutter问题]
---
[csdn上原博文](https://blog.csdn.net/email_jade/article/details/85317859###)

【原创不易，转载请注明出处：https://blog.csdn.net/email_jade/article/details/85317859】
<!--more-->
1. 使用AppBar后如何去掉左边的返回箭头。左边的图标对应的是leading，源代码如下（吐槽一下，CSDN暂不支持dart语言）：

    Widget leading = widget.leading;
    if (leading == null && widget.automaticallyImplyLeading) {
      if (hasDrawer) {
        leading = IconButton(
          icon: const Icon(Icons.menu),
          onPressed: _handleDrawerButton,
          tooltip: MaterialLocalizations.of(context).openAppDrawerTooltip,
        );
      } else {
        if (canPop)
          leading = useCloseButton ? const CloseButton() : const BackButton();
      }
    }
    if (leading != null) {
      leading = ConstrainedBox(
        constraints: const BoxConstraints.tightFor(width: _kLeadingWidth),
        child: leading,
      );
    }
修改方式为， leading为null，automaticallyImplyLeading为false：

appBar: AppBar(
        leading: null,
        automaticallyImplyLeading: false,)
2. 使用flutter的canvas做文字绘制的时候用到的api为TextPainter

3. 使用flutter绘制控件的时候想做到控件超出屏幕范围后自动换行，那么请参考Wrap，可以轻松实现如下的布局：



4. 要实现类似安卓原生ViewPager的UI，请使用PageView，注意定义自己的PageController，然后可以利用PageController的jumpToPage（int）实现自定义的Page页的跳转

5. 要实现类似顶部和底部导航栏，请参考TabBar，适当的时候可以和AppBar结合使用

6. flutter is a SingleTickerProviderStateMixin but multiple tickers were created. 报错，原因是多个地方调用setState请求重绘，但是state使用的是SingleTickerProviderStateMixin ，将其改成TickerProviderStateMixin即可。

7. 解决类冲突的问题，比如，我自定义一个Banner.dart类，这个类跟系统的Banner冲突，那么我们可以这样解决。

import 'package:flutter/material.dart';
import 'package:myproject/Banner.dart' as myproject;
 
//这样使用我们自己的Banner
myproject.Banner _myBanner;
//系统的Banner
Banner _banner;
8. 解决Android手机布局浸入到状态栏的问题，用一个SafeArea进行包装即可，如下：

SafeArea(top: true,
      child: MaterialApp(
        home: ,
      ),);
9. 在切换tabbar或者pageview的时候要保存上一个tab widget的状态，参考AutomaticKeepAliveClientMixin既可，如下：

//假如PageView有四个子页面
 
 @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: PageView(
          controller: pageController,
          children: <Widget>[
            ArticlesPage(),
            ProjectPage(),
            NavigationPage(),
            CollectionArticlesPage(),
          ],
          onPageChanged: changePage,
        ),
        bottomNavigationBar: Navigations(_page, changePage));
  }
 
 
 
//然后在子Page的State分别实现with AutomaticKeepAliveClientMixin，wantkeepAlive返回true
 
class ArticlesPageState extends State<ArticlesPage> with AutomaticKeepAliveClientMixin{
  @override
  bool get wantKeepAlive => true;
}
 
class ProjectPageState extends State<ProjectPage> with AutomaticKeepAliveClientMixin{
  @override
  bool get wantKeepAlive => true;
}
 
class NavigationPageState extends State<NavigationPage> with AutomaticKeepAliveClientMixin{
  @override
  bool get wantKeepAlive => true;
}
 
class CollectionArticlesPageState extends State<CollectionArticlesPage> with AutomaticKeepAliveClientMixin{
  @override
  bool get wantKeepAlive => true;
}
10. Android手机启动时候白屏的问题解决，android/app/src/main/res/drawable/launch_background.xml中定义了自定义splash的方法：

<?xml version="1.0" encoding="utf-8"?>
<!-- Modify this file to customize your launch splash screen -->
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@android:color/white" />
 
    <!-- You can insert your own image assets here -->
    <!-- <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/launch_image" />
    </item> -->
</layer-list>
将<item>注释去掉，替换为自己的launcher_image即可 。

11.  界面存在输入框的时候，点击后软键盘将页面顶起来导致页面重绘的问题（Android fitsystem），可以通过将Scaffold的resizeToAvoidBottomPadding属性设置为false来关闭重绘，如下：

return Scaffold(
      resizeToAvoidBottomPadding: false,
);
12. 修改TextFiled的边界宽度，可以通过decoration的contentPadding属性进行修改，如下：

return TextField(
        decoration: InputDecoration(
          contentPadding: EdgeInsets.all(8),
        ),
);
13. 如果想实现一个布局，在某些条件下显示，可以采用Offstage布局，动态控制其offstage属性值即可

14. 如果出现弹出输入法的时候导致Overflow错误，可以将布局镶嵌到SingleChildScrollView中，比如：

 return Scaffold(
      body: SingleChildScrollView(
        child: Container(
          constraints: BoxConstraints(
            maxHeight: MediaQuery.of(context).size.height,
            maxWidth: MediaQuery.of(context).size.width,
          ),
        ),
    ),
);
15. GridView的item宽高默认是1:1，可以通过修改childAspectRatio的值来进行宽高的修改，该值代表宽：高

16. flutter中绘制虚线，使用path_drawing

17. flutter 中禁用GridView的滚动，可以使用physics属性，取值为NeverScrollableScrollPhysics()，如下：

GridView.count(
      physics: NeverScrollableScrollPhysics(),
);
18. flutter隐藏状态栏，使用：

SystemChrome.setEnabledSystemUIOverlays([]);
19. 监听某个widget是否已经渲染完成，使用WidgetsBinding，方法是在initstate或者build中注册回调，如下：

    WidgetsBinding.instance.addPostFrameCallback((callback){
      print("complete");
    });
20. flutter设置屏幕支持的方向：

   以下设置为设置整个项目运行到时候只允许横屏，如果需要其他方向，可以参考设置。

  SystemChrome.setPreferredOrientations([DeviceOrientation.landscapeLeft, DeviceOrientation.landscapeRight]).then((_){
    runApp(MyApp());
  });
对于IOS来说，可能我们设置只允许横屏了，但是效果确依旧可以竖屏，记得修改xcode的General--Deployment Info--Device Orientation属性，自己勾选对应的方向，如下。



设置屏幕显示方向，由于flutter中有bug，在IOS端可能不生效，需要插件支持，见 https://github.com/jadennn/flutter_orientation 

21. flutter设置多语言支持的时候发现在IOS端只显示英语的bug，是由于xcode中默认没有添加中文（其他语言类似）的选择，解决办法，在Info--Locallzations中选择需要的语言，如下：



22. flutter中禁止控件复用，可以使用不同的key，比如说，如果我们有一个stateful的控件，在initstate中进行了一些值的初始计算，在页面中需要展示多个这样的控件，不想多个控件公用同一套参数（换句话说，initstate只会在第一次初始化的时候调用），那么可以设置不同的key。

23. 裁剪图片的方法：

import 'dart:ui' as DartUi;
  
///根据src和dst裁剪图片
  static DartUi.Image getCroppedImage(DartUi.Image image, Rect src, Rect dst) {
    var pictureRecorder = new DartUi.PictureRecorder();
    Canvas canvas = new Canvas(pictureRecorder);
    canvas.drawImageRect(image, src, dst, Paint());
    return pictureRecorder
        .endRecording()
        .toImage(dst.width.floor(), dst.height.floor());
  }
24. 在不使用BuildContext的情况下进行页面跳转：

    a. 创建一个global的key

static GlobalKey<NavigatorState> gNavigatorKey = new GlobalKey<NavigatorState>();
    b. 在MaterialApp初始化的时候使用

return MaterialApp(
      navigatorKey: Global.gNavigatorKey,
      routes: <String, WidgetBuilder> {
        '/login': (BuildContext context) => new LoginPage(),
      },
      //....代码省略
    c. 需要的地方使用：

Global.gNavigatorKey.currentState.pushNamedAndRemoveUntil('/login',(_) => false);
    注意的是这种方法代价比较大，除非特殊情况，否则不建议使用。使用的时候根据不同的场景调用不同的push方法
--------------------- 
作者：Hirabbit_jaden 
来源：CSDN 
原文：https://blog.csdn.net/email_jade/article/details/85317859 
版权声明：本文为博主原创文章，转载请附上博文链接！