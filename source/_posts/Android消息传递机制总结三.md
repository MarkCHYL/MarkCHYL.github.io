---
layout: post
title: Android消息传递机制总结(三)
toc: true
reward: true
date: 2020-10-10 15:25:12
tags: [消息通信]
categories: [Android]
---
第三方通信 ——— EventBus，rxBus
### 3. 第三方通信

EventBus主要角色:
> Event 传递的事件对象
 Subscriber  事件的订阅者 
 Publisher  事件的发布者
 ThreadMode 定义函数在何种线程中执行
<!-- more -->
定义一个事件类型
```
public class DataEvent {
    private int count;

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }
}
```
```
//订阅

EventBus.getDefault().register(this);//订阅

 //解除订阅

EventBus.getDefault().unregister(this);//解除订阅

//发布事件

EventBus.getDefault().post(new DataEvent());

//订阅事件处理

    @Subscribe(threadMode = ThreadMode.MAIN) //在ui线程执行
    public void onDataEvent(DataEvent event) {
        Log.e(TAG, "event---->" + event.getCount());
    }
```
#### ThreadMode总共四个：
```
NAIN UI主线程
BACKGROUND 后台线程
POSTING 和发布者处在同一个线程
ASYNC 异步线程
```
#### 事件的优先级类似广播的优先级，优先级越高优先获得消息
 ```
 @Subscribe(threadMode = ThreadMode.MAIN,priority = 100) //在ui线程执行 优先级100
    public void onDataEvent(DataEvent event) {
        Log.e(TAG, "event---->" + event.getCount());
    }
```

#### 终止事件往下传递
发送有序广播可以终止广播的继续往下传递，EventBus也实现了此功能
```
 EventBus.getDefault().cancelEventDelivery(event) ;//优先级高的订阅者可以终止事件往下传递
 ```
#### EventBus黏性事件
何为黏性事件呢？简单讲，就是在发送事件之后再订阅该事件也能收到该事件，跟黏性广播类似。

本身粘性广播用的就比较少，为了方便理解成订阅在发布事件之后，但同样可以收到事件。订阅/解除订阅和普通事件一样，但是处理订阅函数有所不同，需要注解中添加sticky = true

```
  @Subscribe(threadMode = ThreadMode.MAIN,sticky = true) //在ui线程执行
    public void onDataEvent(DataEvent event) {
        Log.e(TAG, "event---->" + event.getCount());
    }

//发送粘性事件

  EventBus.getDefault().postSticky(new DataEvent());

//对于粘性广播我们都比较清楚属于常驻广播，对于EventBus粘性事件也类似，我们如果不再需要该粘性事件我们可以移除

  EventBus.getDefault().removeStickyEvent(new DataEvent());

//或者调用移除所有粘性事件

  EventBus.getDefault().removeAllStickyEvents();


```
RXBus：如果项目中用了rxjava的话可以参考https://github.com/AndroidKnife/RxBus 自己封装一个。

### [下一章长链接推送](http://markchyl.cn/2020/10/12/Android%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93-%E5%9B%9B/)