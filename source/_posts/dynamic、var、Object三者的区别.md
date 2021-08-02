---
layout: post
title: dynamic、var、Object三者的区别
toc: true
reward: true
date: 2020-09-23 10:50:17
tags: [dart]
categories: [flutter]
---
### dynamic：
是所有Dart对象的基础类型， 在大多数情况下，通常不直接使用它，
通过它定义的变量会关闭类型检查，这意味着 
`dynamic x = 'hal';x.foo();`
这段代码静态类型检查不会报错，但是运行时会crash，因为x并没有foo()方法，所以建议大家在编程时不要直接使用dynamic；
### var：
是一个关键字，意思是“我不关心这里的类型是什么。”，系统会自动推断类型runtimeType；
### Object：
是Dart对象的基类，当你定义：Object o=xxx；时这时候系统会认为o是个对象，你可以调用o的toString()和hashCode()方法
因为Object提供了这些方法，但是如果你尝试调用o.foo()时，静态类型检查会进行报错；

### 总结：
综上不难看出dynamic与Object的最大的区别是在静态类型检查上；