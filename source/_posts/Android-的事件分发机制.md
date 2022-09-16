---
layout: post
title: Android 的事件分发机制
toc: true
reward: true
date: 2020-12-30 10:44:33
tags: [基础理论]
categories: [Android]
---
Android 事件分发总是遵循 Activity => ViewGroup => View 的传递顺序；
从按下开始依次从Activity开始处理，然后向下分发到ViewGroup，再到最下面的View。

一般情况下，事件列都是从用户按下（ACTION_DOWN）的那一刻产生的，
负责对事件进行分发的方法主要有三个，分别是：
* dispatchTouchEvent()
* onTouchEvent()
* onInterceptTouchEvent()
  
事件：事件就是Event
事件类型分为四种
* DOWN是按下
* UP是抬起
* MOVE是滑动
* CANCEL是取消事件

它们并不存在于所有负责分发的组件中，其具体情况总结于下面的表格中：
dispatchTouchEvent,onTouchEvent方法存在于上文的三个组件中。而onInterceptTouchEvent为ViewGroup独有。
ViewGroup类中，实际是没有onTouchEvent方法的，但是由于ViewGroup继承自View，而View拥有onTouchEvent方法，故ViewGroup的对象也是可以调用onTouchEvent方法的。
