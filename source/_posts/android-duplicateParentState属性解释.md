---
layout: post
title: 'android:duplicateParentState属性解释'
toc: false
reward: false
date: 2019-12-31 11:01:21
tags: [布局]
categories: [Android]
---

**android:duplicateParentState指的是当前控件是否跟随父控件的(点击、焦点等)状态**
>例：假设一Layout有两子View，对Layout进行监听点击事件；子ViewA一个设置duplicateParentState为true，子ViewB设置为false，当点击Layout后，子ViewA的点击态背景变色成功，子ViewB背景态变色无效，因为点击事件被Layout捕获。
<!-- more -->

————————————————

原文链接：https://blog.csdn.net/fancylovejava/article/details/38039847