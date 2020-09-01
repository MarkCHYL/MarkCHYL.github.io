---
layout: post
title: Flutter中使用Banner 图
date: 2019-04-10 15:05:57
toc: true
reward: true
tags: [Flutter]
categories: [Flutter]
---
## 实现效果
!["效果图"](https://img-blog.csdnimg.cn/20190410142106370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01hcmtfQ0hZTA==,size_16,color_FFFFFF,t_70 "效果图")
 <!--more-->
## 首先导入flutter_swiper插件：
```
dependencies:
  flutter_swiper: ^1.1.6
```
[配置文件的链接地址](https://pub.dartlang.org/packages/flutter_swiper#-installing-tab-)
## 使用swiper插件
需要在使用的dart页面添加：
```
import 'package:flutter_swiper/flutter_swiper.dart';
```
然后才能在页面中使用轮播插件：
```
body:  new Swiper(
        itemBuilder: (BuildContext context,int index){
          return new Image.network("http://via.placeholder.com/350x150",fit: BoxFit.fill,);
        },
        itemCount: 3,
        pagination: new SwiperPagination(),
        control: new SwiperControl(),
      ),
    );
 ```
 