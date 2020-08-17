---
layout: post
title: ScrollView与RecyclerView解决滑动冲突
date: 2019-09-02 13:04:32
tags: [Android,冲突]
---
[原文](https://www.jianshu.com/p/3e0ad704bee5)

**RecyclerView 嵌套RecyclerView是不存在冲突的 因为内部已经解决，ScrollVIew嵌套RecyclerView会存在现实不全的问题，滑动的时候会导致不流畅，所以我换用的是NestedScrollView嵌套RecyclerView不会存在显示不完全的现象，但是还会有一点的滑动冲突**

 <!--more-->
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.NestedScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="标题党"/>

        <android.support.v7.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>
      
    </LinearLayout>

</android.support.v4.widget.NestedScrollView>
```

**上面只是布局代码而已，滑动冲突还是没有解决，我们只要在recyclerView 设置一下这个属性就可以了**
    `recyclerView.setNestedScrollingEnabled(false);`
