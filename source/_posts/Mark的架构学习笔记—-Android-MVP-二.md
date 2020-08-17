---
layout: post
title: Mark的架构学习笔记— Android_MVP(二)
date: 2019-01-25 09:32:40
toc: true
reward: true
tags: [Android,架构]
---
### 前言
之前我写的一个笔记，没有结合网络请求，现在我想记录下我这次结合网络请求Retrofit的MVP编程。
我上一篇的笔记：[马克的架构学习笔记— Android_MVP](https://markchyl.github.io/2018/12/17/Mark%E7%9A%84%E6%9E%B6%E6%9E%84%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-Android-MVP/),代码笔记中有链接。

根据慕课网上的学习的代码链接：[根据手机号查询号码信息](https://github.com/MarkCHYL/MarkTellNumInfo_mvp)

这篇笔记我不贴代码，制作个笔记记录，项目代码不能外泄。
<!--more-->
### 老话重提，啥事MVP模式？
这个问题是真的很重要，MVP就是一个编程思想，让代码具有层次感，逻辑鲜明。

在MVP里，Presenter完全把Model和View进行了分离，主要的程序逻辑在Presenter里实现。而且，Presenter与具体的View是没有直接关联的，而是通过定义好的接口进行交互，从而使得在变更View时候可以保持Presenter的不变，即重用！ 不仅如此，我们还可以编写测试用的View，模拟用户的各种操作，从而实现对Presenter的测试--而不需要使用自动化的测试工具。

* ### 优点
  * 模型与视图完全分离，我们可以修改视图而不影响模型
  * 可以更高效地使用模型，因为所有的交互都发生在一个地方——Presenter内部
  * 我们可以将一个Presenter用于多个视图，而不需要改变Presenter的逻辑。这个特性非常的有用，因为视图的变化总是比模型的变化频繁。
  * 如果我们把逻辑放在Presenter中，那么我们就可以脱离用户接口来测试这些逻辑（单元测试）
  *  (1)降低耦合度

     (2)模块职责划分明显

     (3)利于测试驱动开发

     (4)代码复用

     (5)隐藏数据

     (6)代码灵活性
     
     
* ### 缺点
  由于对视图的渲染放在了Presenter中,所以视图和Presenter的交互会过于频繁。还有一点需要明白,如果Presenter过多地渲染了视图,往往会使得它与特定的视图的联系过于紧密。一旦视图需要变更,那么Presenter也需要变更了

### 大白话
* View层就是用于自己显示的，你需要什么样的显示接口，就可以在View层中定义。
* Presenter层就是协调View和Model层的，可以将网络请求和接 main 显示的方法调用放在P层，然后调用model层的方法，我这一般就是保存数据和获取数据。
* model其实就是数据管理类

![image](https://note.youdao.com/yws/api/personal/file/WEB3a6a0577e5958e93f8a25e14eade5b77?method=download&shareKey=f7d60e8b528af13a443d9b56d1d4a6ab)