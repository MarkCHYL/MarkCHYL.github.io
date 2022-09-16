---
layout: post
title: Android如何进行技术选型（案列：Navigation组件和ARouter为例）
toc: true
reward: true
date: 2022-09-16 23:01:01
tags: [Android,架构师]
categories: [Android]
---
## 案列：Navigation组件和ARouter为例
[TOC]

[我的Demo：](https://gitee.com/markshow/hirouter)涵盖了两大路由的基础使用

### 如何做好技术选型？

![././marksource/images/OPlayer_2022-07-12_18-19-20.jpg](../../marksource/images/OPlayer_2022-07-12_18-19-20.jpg)
### 什么是路由？
* 页面间跳转关系映射，可以通过字符串、别名等方式实现跳转
### 为什么介入路由？Intent不好吗？
**Intent**
* 跳转过程无法控制，一旦调用了startActivity(Intent)便交由系统执行，中间过程无法插手
* 跳转失败无法捕获、降级，出现问题直接抛出异常
* 显示Intent
    显示Intent中因为存在直接的类依赖关系，导致耦合严重
* 隐式Intent
隐式Intent中会出现规则集中式的管理，导致协作困难，都需要在Manifest中进行配置，导致扩展性比较差
### 对比两大路由组件的功能对照表

|  | Navigation | ARouter |
| --- | --- | --- |
| 跳转行为 | 通过页面的action跳转，支持Activity，Fragment，Dialog |  支持标准URL跳转|
| 模块间通信 | 不支持，需要将所有页面定义在一个资源文件里 | @Route注解配置，根据Path获取对应的接口实现 |
| 路由节点注册 | 统一在navigation_mobile.xml中注册 | @Route注解 |
| 路由节点的生成方式 | 加载navigation_mobile.xml生成NavGraph导航视图 | 按照组划分 |
| 拦截器 | 不支持 | 支持全部定义拦截器，可以自定义拦截顺序 |
| 转场动画 | 支持 | 支持 |
| 降级策略 | 不支持 | 支持全局降级和局部降级 |
| 参数自动注入 | 不支持 | @Autowired注解实现 |
| 外部跳转控制（h5打开app页面） | deeplink页面直达 | 需要配置入口Activity，支持的uri需要在Manifest中配置|

### 总结
**ARouter主要是用于Activity路由的框架，采用的是APT技术，可用于组件化改造。而Navigation主要是用于Fragment路由导航的框架。**
> 可以说，这两个虽然都是用于路由导航，但是用途是不一样。如果你的应用主要以Acticity为主的话，建议使用ARouter；如果你的页面主要以Fragment为主，Activity只是作为容器的话，那么建议你使用Navigation。当然，你也可以两个都用，这完全取决于你的项目实际情况，不冲突的。
 
[Android中APT技术介绍](https://app.yinxiang.com/shard/s50/nl/22109192/56fe3693-8eb4-4574-a55b-a42851e4e89c/)