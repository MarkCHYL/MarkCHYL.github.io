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

[下一章组件间通信](http://markchyl.cn/2020/10/10/Android%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93%E4%BA%8C/)