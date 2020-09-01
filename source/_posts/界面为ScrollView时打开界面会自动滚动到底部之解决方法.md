---
layout: post
title: 界面为ScrollView时打开界面会自动滚动到底部之解决方法
date: 2019-09-02 13:40:15
tags: [Android,冲突]
categories: [Android]
---
版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
本文链接：https://blog.csdn.net/u014449096/article/details/72885947

**开发中遇到了这样的一个问题，界面最外层是ScrollView，然后里面有嵌套了一个ListView还有其他可以获取焦点的View，然后每次打开界面都会自动滚动到最底部，经过一番折腾，发现了一个简单的方法,
获取ScrollView里面一个上层任意view，然后调用如下方法：**
 <!--more-->
`view.setFocusable(true); 
view.setFocusableInTouchMode(true); 
view.requestFocus();`

**或者将scrollview包裹的内容设置上以下两个属性 **
`android:focusable=”true”
android:focusableInTouchMode=”true”`

例如：
```
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:scrollbars="none"
    >
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:focusable="true"
        android:focusableInTouchMode="true" />
        
</ScrollView
```