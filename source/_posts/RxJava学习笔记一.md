---
layout: post
title: RxJava学习笔记一
toc: true
reward: true
date: 2020-10-19 16:47:15
tags: [RxJava]
categories: [RxJava]
---
### Hello World!
* RxJava1.1.7.jar
代码演示：
```
public static void main(String[] args) {
		//第一步、创建被观察者
		Observable<String> mObservable = Observable.create(new OnSubscribe<String>(){
			@Override
			public void call(Subscriber<? super String> subscriber) {
				subscriber.onNext("Hello World!");
				subscriber.onCompleted();
			}
		});
		
		//第二步、创建观察者
		Subscriber<String> mSubscriber = new Subscriber<String>() {

			@Override
			public void onCompleted() {
				// TODO Auto-generated method stub
				System.out.println("onCompleted");
			}

			@Override
			public void onError(Throwable arg0) {
				// TODO Auto-generated method stub
				System.out.println("onError");
			}

			@Override
			public void onNext(String arg0) {
				// TODO Auto-generated method stub
				System.out.println("OnNext:"+arg0);
				
			}
		};
		
		//第三步、订阅事件
		mObservable.subscribe(mSubscriber);
	}
```
> 例如代码中所演示的，RxJava的使用就是那么简单，通过订阅事件将被观察者和观察者进行绑定。
<!-- more -->
### 响应式编程：
在介绍RxJava前，我们先聊聊响应式编程。那么什么是响应式编程呢？响应式编程是一种基于异步数据流概念的编程模式。数据流就像一条河：它可以被观测，被过滤，被操作，或者为新的消费者与另外一条流合并为一条新的流。

响应式编程的一个关键概念是事件。事件可以被等待，可以触发过程，也可以触发其它事件。事件是唯一的以合适的方式将我们的现实世界映射到我们的软件中：如果屋里太热了我们就打开一扇窗户。同样的，当我们的天气app从服务端获取到新的天气数据后，我们需要更新app上展示天气信息的UI；汽车上的车道偏移系统探测到车辆偏移了正常路线就会提醒驾驶者纠正，就是是响应事件。

今天，响应式编程最通用的一个场景是UI：我们的移动App必须做出对网络调用、用户触摸输入和系统弹框的响应。在这个世界上，软件之所以是事件驱动并响应的是因为现实生活也是如此。
>本章节中部分概念摘自《RxJava Essentials》一书

### RxJava简介：
RxJava本质上是一个异步操作库，是一个能让你用极其简洁的逻辑去处理繁琐复杂任务的异步事件库。

Rx是微软.Net的一个响应式扩展，Rx借助可观测的序列提供一种简单的方式来创建异步的，基于事件驱动的程序。2012年Netflix为了应对不断增长的业务需求开始将.NET Rx迁移到JVM上面。并于13年二月份正式向外展示了RxJava。
从语义的角度来看，RxJava就是.NET Rx。从语法的角度来看，Netflix考虑到了对应每个Rx方法,保留了Java代码规范和基本的模式。
![](https://pic1.zhimg.com/80/ca736fe59de94c221875cb428d7eaff0_1440w.png)

### RxJava好在哪
* 供了AsyncTask,Handler等用来做异步操作的类库了AsyncTask,Handler等用来做异步操作的类库
* 非常简洁的代码逻辑来解决复杂问题