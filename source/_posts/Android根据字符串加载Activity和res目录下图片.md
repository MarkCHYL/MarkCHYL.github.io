---
layout: post
title: Android根据字符串加载Activity和res目录下图片
toc: true
reward: true
date: 2021-01-15 16:51:12
tags: [Android]
categories: [Android]
---
### 根据传入的字符串跳转Activity
```
Intent intent = new Intent(context,Class.forName("com.packname.Activity"));
startActivity(intent);
```
### 根据传入的字符加载资源
```
int icon = getResources().getIdentifier(“imageid”, "drawable",getPackageName());
```
优化：
```
public static int getDrawableId(Context context, String var) {

        try {
            int imageId = context.getResources().getIdentifier(var, "drawable", context.getPackageName());
            return imageId;
        } catch (Exception e) {
            return 0;
        }
    }
```
**getIdentifier的函数签名如下：**
```
public int getIdentifier (String name, String defType, String defPackage)
```