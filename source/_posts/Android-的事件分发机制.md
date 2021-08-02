---
layout: post
title: Android 的事件分发机制
toc: true
reward: true
date: 2020-12-30 10:44:33
tags: [基础理论]
categories: [Android]
---

一般情况下，事件列都是从用户按下（ACTION_DOWN）的那一刻产生的，
负责对事件进行分发的方法主要有三个，分别是：
* dispatchTouchEvent()
* onTouchEvent()
* onInterceptTouchEvent()
  
它们并不存在于所有负责分发的组件中，其具体情况总结于下面的表格中：
dispatchTouchEvent,onTouchEvent方法存在于上文的三个组件中。而onInterceptTouchEvent为ViewGroup独有。
ViewGroup类中，实际是没有onTouchEvent方法的，但是由于ViewGroup继承自View，而View拥有onTouchEvent方法，故ViewGroup的对象也是可以调用onTouchEvent方法的。
