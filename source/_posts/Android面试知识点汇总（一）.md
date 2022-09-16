---
layout: post
title: Android面试知识点汇总（一）
toc: true
reward: true
date: 2021-09-06 17:25:52
tags: [面试]
categories: [面试]
---
### 一、Activity启动模式
* 1、standard 标准模式
* 2、singleTop 栈顶复用模式 （例如：推送点击消息页面）
* 3、singleTask 栈内复用模式 （例如： 首页）
* 4、singleInstance 单例模式 （单独位于一个任务栈中，例如：拨打电话页面）

### 二、序列化 Serializable 和 Parcelable

* Serializable： Java 序列化方式，适用于存储和网络传输，serializableUID 用于确定反序列化和类版本是否一致，不一致时反序列化失败
* Parcelable: Android 序列化方式，适用于组件通信数据传输，性能高

Java中的序列化方式Serializable效率比较低，重要是以下原因：
* 1、Serializable在序列化过程中会创建大量的临时变量，这样会造成大量的GC
* 2、Serializable使用了大量的反射，而反射操作耗时
* 3、Serializable使用了大量的IO操作，也影响耗时

所以Android重新设计了一种序列化的方式，结合Binder的方式，对上述三点进行了优化，一定程度上提高了序列化和返序列化的效率。

### 三、进程知识点
#### **IPC进程通信**

  * Intent、Bundle：要求传输数据能被序列化，实现 Parcelable、 Serializable，适用于四大组件通信
  * 文件共享：适用于交换简单的数据实时性不高的场景
  * AIDL：AIDL 接口实质上是系统提供给我们可以方便实现Binder的工具
  * Messenger：基于 AIDL 实现，服务端的串行处理，主要用于传递消息，适用于低并发一对多的通信
  * ContentProvider： 基于 Binder 实现，适用于一对多的进程间的数据共享。（通讯录、短信等）
  * Socket：TCP、UDP，适用于网络数据交换

#### **进程保活**
  进程被杀的原因：
  * 切换到后台内存不足时被杀
  * 切换到后台厂商省电机制杀死
  * 用户主动清理

  保活方式：
  * 1、 Activity提权：挂一个 1像素 Activity将进程优先级提高到前台进程
  * 2、 Service提权： 启动一个前台服务，（API>18会有正在运行的通知栏）
  * 3、 广播拉活。（监听 开机 等系统广播）
  * 4、 Service拉活
  * 5、 JobScheduler 定时任务拉活。（Android 高版本不行）
  * 6、 双进程保活
  * 7、 监听其他大厂广播 （如tx baidu 全家桶互相拉活）

### 4、内存泄露
* 构造单列的时候尽量别用Activity的引用；
* 静态引用时注意应用对象的置空或者少用静态引用；
* 使用静态内部类+软引用代替非静态内部类；
* 即使取消广播或者观察者注册、耗时任务、属性动画在Activity销毁时记得cancel；
* 文件流、Cursor等资源操作即使关闭；
* Activity销毁时WebView的移除和销毁。

### 5、Android进程间的通信
* bundle：由于Activity、service、receiver都是可以通过 Intent 来携带Bundle数据传输的，所以我们在一个进程中通过Intent将携带数据的Bundle发送到另一个进程的组件；（bundle只能传输三种数据类型，一是键值对的形式，二是键为String的类型，三是值为Parcelable类型）
* ContentProvider： contentprovicer是安卓四大组件之一，以表格 的形式来存储数据，提供给外界，及ContentProvider 可以跨进程访问其他应用程序中的数据。
* 文件：两个进程可以到同一个文件去交流数据，我们不仅可以保存文本文件，还可以将对象持久化到文件中，从另一个文件恢复。要注意的是，当并发读/写时可能出现并发的问题。
* Broadcast：Broadcast可以想Android系统中所有的应用程序发送广播，需要跨进程通信的应用程序可以监听这些广播。
* AIDL： AIDL通过定义服务端暴露的接口，以提供给客户端来通用，AIDL使用服务器可以并行处理。
* Messenger：Messenger封装了AIDL之后只能串行运行，所以Messenger一般作用消息传递
* Socket：

### 6、Android 线程通信
>Handler 和 AsyncTask （AsyncTask：异步任务，内部封装了Handler）

Handler线程间通信
作用：
>线程之间的消息通信

流程：

>主线程默认实现了Looper （调用loop.prepare方法 向sThreadLocal中set一个新的looper对象， looper构造方法中又创建了MsgQueue） 手动创建Handler ，调用 sendMessage 或者 post (runable) 发送Message 到 msgQueue ，如果没有Msg 这添加到表头，有数据则判断when时间 循环next 放到合适的 msg的next 后。Looper.loop不断轮训Msg，将msg取出 并分发到Handler 或者 post提交的 Runable 中处理，并重置Msg 状态位。回到主线程中 重写 Handler 的 handlerMessage 回调的msg 进行主线程绘制逻辑。

问题：

1、Handler 同步屏障机制：通过发送异步消息，在msg.next 中会优先处理异步消息，达到优先级的作用。

2、Looper.loop 为什么不会卡死：为了app不挂掉，就要保证主线程一直运行存在，使用死循环代码阻塞在msgQueue.next()中的nativePollOnce()方法里 ，主线程就会挂起休眠释放cpu，线程就不会退出。Looper死循环之前，在ActivityThread.main()中就会创建一个 Binder 线程（ApplicationThread），接收系统服务AMS发送来的事件。当系统有消息产生（其实系统每 16ms 会发送一个刷新 UI 消息唤醒）会通过epoll机制 向pipe管道写端写入数据 就会发送消息给 looper 接收到消息后处理事件，保证主线程的一直存活。只有在主线程中处理超时才会让app崩溃 也就是ANR。

3、Messaage复用：将使用完的Message清除附带的数据后, 添加到复用池中 ,当我们需要使用它时,直接在复用池中取出对象使用,而不需要重新new创建对象。复用池本质还是Message 为node 的单链表结构。所以推荐使用Message.obation获取 对象。


[剩余链接](https://mp.weixin.qq.com/s/dyCEnsdUo-AGhqeCzN0E5w)