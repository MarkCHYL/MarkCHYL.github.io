---
layout: post
title: Android 架构MVC MVP MVVM
toc: true
reward: true
date: 2021-09-01 10:08:00
tags: [Android,架构]
categories: [Android]
---
## MVVM是什么
>是 Model-View-ViewModel 的简写。MVVM与MVP的结构还是很相似的，就是将Presenter升级为ViewModel。在MVVM中，View层和Model层进行了双向绑定(即Data Binding)，所以Model数据的更改会表现在View上，反之亦然。ViewModel就是用来根据具体情况处理View或Model的变化。
### Android中的MVVM含义
* **Model**：实体类(数据的获取、存储、数据状态变化)。
* **View**：布局文件+Activity。
* **ViewModel**： 关联层，将Model和View进行绑定，Model或View更改时，实时刷新对方。
  
工作原理
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4cb68983949e49fe93a6d31aa8c2bcd9~tplv-k3u1fbpfcp-watermark.awebp)

* 1.View 接收用户交互请求
* 2.View 将请求转交给ViewModel
* 3.ViewModel 操作Model数据更新
* 4.Model 更新完数据，通知ViewModel数据发生变化
* 5.ViewModel 更新View数据

### MVVM的优点
* 1.提高可维护性。Data Binding可以实现双向的交互，使得视图和控制层之间的耦合程度进一步降低，分离更为彻底，同时减轻了Activity的压力。
* 2.简化测试。因为同步逻辑是交由Binder做的，View跟着Model同时变更，所以只需要保证Model的正确性，View就正确。大大减少了对View同步更新的测试。
* 3.ViewModle易于单元测试。

### MVVM的缺点
* 1.对于简单的项目，使用MVVM有点大材小用。
* 2.对于过大的项目，数据绑定会导致内存开销大，影响性能。
* 3.ViewModel和View的绑定，使页面异常追踪变得不方便。有可能是View出错，也有可能是ViewModel的业务逻辑有问题，也有可能是Model的数据出错。

## MVP和MVC的最大区别
>在MVP中View并不直接使用Model，它们之间的通信是通过Presenter 来进行的，所有的交互都发生在Presenter内部，而在MVC中View直接从Model中读取数据而不是通过 Controller。

## 如何选取框架
适合自己的才是最好的