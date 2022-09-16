---
layout: post
title: Android中APT技术介绍
toc: true
reward: true
date: 2022-09-16 22:59:58
tags: [Android,APT]
categories: [Android]
---
## Android中APT技术介绍
[TOC]
### APT是什么
`APT` 全称 ```Annotation Processing Tool```，即注解处理器，是javac的一种处理注释的工具，它对源代码文件进行检测找出其中的Annotation，并根据注解自动生成代码，帮助开发者减少了很多重复代码的编写。
### 例子
很多著名的框架用到APT的思想，通过注解编译期间自动生成代码，简化使用

1. `Butterknife`
2. `Dragger`
3. `Room`

另外，还有是运行时注解，例如
1. `Retrofit`

### APT有什么用 （好处）
可以在编译时生成额外的.java文件，在程序运行的时候调用相关方法，可以达到减少重复代码的效果。它的好处：提高开发效率，使得项目更容易维护和扩展，同时几乎不影响性能。
### APT原理 （为什么）
通过APT（Annotation Processing Tool）技术，即注解处理器，在编译时扫描并处理注解，注解处理器最终生成处理注解逻辑的.java文件。
### APT实践 （怎么做）
1. 创建自定义注解@interface；
2. 创建并注册注解处理器AbstractProcesso，生成处理注解逻辑的.java文件；
3. 封装一个供外部调用的API，用的是反射技术，具体来说就是调用第二步中生成的代码中的方法；
4. 在业务代码中使用，比如Activity、Fragment、Adapter

#### 参考
1. [Android编译时注解–入门篇（AbstractProcessor、APT）](https://www.jianshu.com/p/b5be6b896a1a)
2. [你必须知道的APT、annotationProcessor、android-apt、Provided、自定义注解](https://blog.csdn.net/xx326664162/article/details/68490059)
3. [Android 注解系列之APT工具](https://juejin.cn/post/6844903701283340301)
4. [Android 利用 APT 技术在编译期生成代码](https://blog.csdn.net/hb707934728/article/details/52213086)
5. [Android注解处理器APT技术简介](https://blog.csdn.net/biaozhiyuan/article/details/117045125)