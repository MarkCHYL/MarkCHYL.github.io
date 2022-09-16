---
layout: post
title: Android消息传递机制总结(一)
toc: true
reward: true
date: 2020-10-09 14:48:16
tags: [消息通信]
categories: [Android]
---
#### 安卓业务层的核心灵魂就是层层的消息传递，今天就来总结一下andorid的应用层的各种消息传递。

1.线程间通讯 ——— Handler，HandlerThread等。
2.组件间通信 ——— BroadcastReceiver，接口回调等。
3. 第三方通信 ——— EventBus，rxBus
4.进程间通信 ——— Content Provider ，Broadcast ，AIDL等。
5.长连接推送 ——— WebSocket，XMPP等。
### 1.线程间通信
Android通过Handler消息机制来实现线程之间的通讯。
<!-- more -->

#### Handler机制主要角色
```
Message：消息，其中包含了消息ID，消息处理对象以及处理的数据等，由MessageQueue统一列队，终由Handler处理。 
Handler：处理者，负责Message的发送及处理。使用Handler时，需要实现handleMessage(Message msg)方法来对特定的Message进行处理，例如更新UI等。 
MessageQueue：消息队列，用来存放Handler发送过来的消息，并按照FIFO规则执行。当然，存放Message并非实际意义的保存，而是将Message以链表的方式串联起来的，等待Looper的抽取。 
Looper：消息泵，不断地从MessageQueue中抽取Message执行。因此，一个MessageQueue需要一个Looper。
Thread：线程，负责调度整个消息循环，即消息循环的执行场所。
```
#### Handler机制主要运用
```
sendEmptyMessage(int);//发送一个空的消息
sendMessage(Message);//发送消息，消息中可以携带参数
sendMessageAtTime(Message, long);//未来某一时间点发送消息
sendMessageDelayed(Message, long);//延时Nms发送消息

post(Runnable);//提交计划任务马上执行
postAtTime(Runnable, long);//提交计划任务在未来的时间点执行
postDelayed(Runnable, long);//提交计划任务延时Nms执行
```
#### 主进程定义Handler
```
 private Handler mHandler = new Handler() {

        @Override
        public void handleMessage(@NonNull Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 0:
                    //完成主界面更新,拿到数据
                    String data = (String) msg.obj;
                    tvTest.setText(data);
                    break;
                default:
                    break;
            }
        }
    };
```
#### 子线程执行耗时操作然后发消息，通知Handler完成UI更新
```
private void getDataFromNet() {
        new Thread(new Runnable() {
            @Override
            public void run() {
              //需要数据传递，用下面方法；
                Message msg = new Message();
                msg.obj = "网络数据";//可以是基本类型，可以是对象，可以是List、map等；
                mHandler.sendMessage(msg);
            }
        }).start();
    }
```
Handler机制扩展：

Activity.runOnUiThread(Runnable)
View.post(Runnable)
以上也可以从子线程切换到主线程。
```
 runOnUiThread(new Runnable() {
            @Override
            public void run() {
                tvTest2.setText("runOnUiThread");
            }
        });
```

#### HandlerThread：
HandlerThread本质上就是一个普通Thread,只不过内部建立了Looper.
HandlerThread用法实例
```
 //创建一个线程,线程名字：handler-thread
        mHThread = new HandlerThread("handler-thread");
        //开启一个线程
        mHThread.start();
        //在这个线程中创建一个handler对象 主要这个handler是在子线程中循环接受消息的
        handler = new Handler(mHThread.getLooper()) {
            @Override
            public void handleMessage(@NonNull Message msg) {
                super.handleMessage(msg);
                //这个方法是运行在 handler-thread 线程中的 ，可以执行耗时操作
                Log.d("handler ", "消息： " + msg.what + "  线程： " + Thread.currentThread().getName());
            }
        };

        //在主线程给handler发送消息
        handler.sendEmptyMessage(1);

        new Thread(new Runnable() {
            @Override
            public void run() {
                //在子线程给handler发送数据
                handler.sendEmptyMessage(2);
            }
        }).start();


        @Override
    protected void onDestroy() {
        super.onDestroy();
        //释放资源
        mHThread.quit();
    }
```

#### Looper的quit方法或quitSafely方法
相同点：
将不在接受新的事件加入消息队列。

不同点
当我们调用Looper的quit方法时，实际上执行了MessageQueue中的removeAllMessagesLocked方法，该方法的作用是把MessageQueue消息池中所有的消息全部清空，无论是延迟消息（延迟消息是指通过sendMessageDelayed或通过postDelayed等方法发送的需要延迟执行的消息）还是非延迟消息。

当我们调用Looper的quitSafely方法时，实际上执行了MessageQueue中的removeAllFutureMessagesLocked方法，通过名字就可以看出，该方法只会清空MessageQueue消息池中所有的延迟消息，并将消息池中所有的非延迟消息派发出去让Handler去处理，quitSafely相比于quit方法安全之处在于清空消息之前会派发所有的非延迟消息。

无论是调用了quit方法还是quitSafely方法只会，Looper就不再接收新的消息。即在调用了Looper的quit或quitSafely方法之后，消息循环就终结了，这时候再通过Handler调用sendMessage或post等方法发送消息时均返回false，表示消息没有成功放入消息队列MessageQueue中，因为消息队列已经退出了。

需要注意的是Looper的quit方法从API Level 1就存在了，但是Looper的quitSafely方法从API Level 18才添加进来。

我在直播推流中应用了HandlerThread，每编码组装出来一帧视频就发送一个handler消息，然后HandlerThread线程接收消息数据并用libRtmp推倒服务器。此时可以监控队列中有多少消息在循环，可以监听进出队列的比例，如果超出一定的范围说明网络不好，需要执行丢帧策略。

### Handler 面试八问
* 为什么主线程不会因为 `Looper.loop()` 里的循环卡死？
    >主线程确实是通过`Looper.loop()` 进入了循环状态，因为这样主线程才不会像我们一般创建的线程一样，当可执行代码执行完后，线程生命周期就终止了。
    在主线程的`MessageQueue` 没有消息时，便阻塞在`MeqsageQueue.next()` 中的`nativePollOnce()`方法里，此时主线程会释放 `CPU`资源进入休眠状态，直到新消息达到。所以主线程大多数时候都是处于休眠状态，并不会消耗大量`CPU`资源。
    这里采用的`linux`的`epoll` 机制，是一种 `IO` 多路复用机制，可以同时监控多个文件描述符，当某个文件描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作拿到最新的消息，进而唤醒等待的线程。

* `post`和`sendMessage`两类发送消息的方法有什么区别?
    >`post`一类的方法发送的是 `Runnable`对象，但是其最后还是会被封装成`Message` 对象，将`Runnable` 对象赋值给 `Message` 对象中的`callback`变量，然后交由 `sendMessageAtTime() `方法发送出去。在处理消息时，会在`dispatchMessage()`方法里首先被`handleCallback(msg)`方法执行，实际上就是执行 `Message` 对象里面的 `Runnable` 对象的run 方法。
    而`sendMessage` 一类的方法发送的直接是`Message`对象，处理消息时，在 `dispatchMessage`里优先级会低于`handleCallback(msg)`方法，是通过自己重写的`handleMessage(msg)` 方法执行。

* 为什么要通过 `Message.obtain()` 方法获取 Message 对象?
    >`obtain `方法可以从全局消息池中得到一个空的` Message`对象，这样可以有效节省系统资源。同时，通过各种`obtain`重载方法还可以得到一些`Message`的拷贝，或对`Message`对象进行一些初始化。

* Handler实现发送延迟消息的原理是什么?
    > 我们常用`postDelayed()`与`sendMessageDelayed()` 来发送延迟消息，其实最终都是将延迟时间转为确定时间，然后通过`sendMessageAtTime()` -> `enqueueMessage` ->
    `queue.enqueueMessage`这一系列方法将消息插入到`MessageQueue`中。所以并不是先延迟再发送消息，而是直接发送消息，再借助`MessageQueue`的设计来实现消息的延迟处理。
    消息延迟处理的原理涉及`MessageQueue`的两个静态方法 `MessageQueue.next()`和
    `MessageQueue.enqueueMessage()`。通过`Native`方法阻塞线程一定时间，等到消息的执行时间到后再取出消息执行。

* 同步屏障 `SyncBarrier`是什么?有什么作用?
    >在一般情况下，同步和异步消息处理起来没有什么不同。只有在设置了同步屏障后才会有差异。同步屏障从代码层面上看是一个`Message` 对象，但是其`target`属性为`null`，用以区分普通消息。在`MessageQueue.next()`中如果当前消息是一个同步屏障，则跳过后面所有的同步消息，找到第一个异步消息来处理。
    但是开发者调用不了。在`ViewRootlmpl`的UI测绘流程有体现

* `IdleHandler` 是什么?有什么作用?
    >当消息队列没有消息时调用或者如果队列中仍有待处理的消息，但都未到执行时间时，也会调用此方法。用以监听主线程空闲状态。

* 为什么非静态类的`Handler`导致内存泄漏?如何解决?
    > 首先，非静态的内部类、匿名内部类、局部内部类都会隐式的持有其外部类的引用。也就是说在 `Activity`中创建的,Handler会因此持有`Activity`的引用。
    当我们在主线程使用`Handler`的时候，Handler会默认绑定这个线程的`Looper`对象，并关联其`MessageQueue，Handler`发出的所有消息都会加入到这个`MessageQueue`中。`Looper`对象的生命周期贯穿整个主线程的生命周期，所以当Looper对象中的MessageQueue里还有未处理完的` Message`时，因为每个`Message`都持有`Handler`的引用，所以`Handler`无法被回收，自然其持有引用的外部类` Activity `也无法回收，造成泄漏。
    **使用静态内部类+弱引用的方式**

* 如何让在子线程中弹出toast
    >调用`Looper.prepare`以及`Looperloop()`，但是切记线程任务执行完，需要手动调用`Looper.quitSafely()`否则线程不会结束。



[下一章: 组件间通信](http://markchyl.cn/2020/10/10/Android%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93%E4%BA%8C/)