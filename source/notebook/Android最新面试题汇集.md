---
layout: post
title: Android面试题汇集
toc: false
reward: false
date: 2020-09-03 13:15:54
tags: [面试]
categories: [面试]
---

## Android基础
### 1、什么是ANR 如何避免它？
ANR 就是一个无响应的对话框,主要原因就是在主线程做了耗时操作。
>全称为Application Not Responding。在Android 中，如果你的应用程序有一段时间没有响应，系统会向用户显示一个对话框，这个对话框称作应用程序无响应对话框。
不同的组件发生ANR 的时间不一样，
* 主线程（Activity）是5 秒;
* BroadCastReceiver 是10 秒;
* Service：20 秒（均为前台）,Service 在20 秒内无法处理完成。

**解决方案：**
* 将所有耗时操作，比如访问网络，Socket 通信，查询大量SQL 语句，复杂逻辑计算等都放在子线程中去，然后通过handler.sendMessage、runonUITread、AsyncTask 等方式更新UI，以确保用户界面操作的流畅度.
* 如果耗时操作需要让用户等待，那么可以在界面上显示进度条。
* 避免在activity里面做耗时操作，oncreate & onresume
* 避免在onReceiver里面做过多操作
* 避免在Intent Receiver里启动一个Activity，因为它会创建一个新的画面，并从当前用户正在运行的程序上抢夺焦点。

### 2、View的绘制流程；自定义View如何考虑机型适配；自定义View的事件
>View的绘制流程：OnMeasure()——>OnLayout()——>OnDraw()
组合控件、继承原有的控件、继承原有的控件
* 第一步：OnMeasure()：测量视图大小。从顶层父View到子View递归调用measure方法，measure方法又回调OnMeasure。
* 第二步：OnLayout()：确定View位置，进行页面布局。从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小和布局参数，将子View放在合适的位置上。
* 第三步：OnDraw()：绘制视图。ViewRoot创建一个Canvas对象，然后调用OnDraw()。六个步骤：绘制视图的背景、保存画布的图层(Layer)、绘制View的内容、绘制View子视图、还原图层(Layer)、绘制滚动条（如果没有就不用）;

### 3、分发机制；View和ViewGroup分别有哪些事件分发相关的回调方法；自定义View如何提供获取View属性的接口；
> 基本会遵从 Activity => ViewGroup => View 的顺序进行事件分发，然后通过调用 onTouchEvent() 方法进行事件的处理。我们在项目中一般会对 MotionEvent.ACTION_DOWN，MotionEvent.ACTION_UP，MotionEvent.ACTION_MOVE，MotionEvent.ACTION_CANCEL 分情况进行操作。


<!-- more -->

4、Art和Dalvik对比；虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)；JVM内存模型及类加载机制；内存对象的循环引用及避免；

### 4、ddms 和 traceView；
* 1， ddms：是android开发环境中的dalvik虚拟机调试监控服务；
ddms能够提供，测试设备截屏，针对特定的进程查看正在运行的线程以及堆信息，Logcat，广播状态信息，模拟电话呼叫，接收sms，虚拟地理坐标等。

* 2，traceView是android平台配备的性能分析的工具；它可以通过图形化让我们了解要跟踪的程序的性能，并且能具体到方法。

区别：ddms是一个程序执行查看器，在里面可以看见线程和堆栈等信息，traceView是程序性能分析器。

### 5、内存回收机制与GC算法(各种算法的优缺点以及应用场景)；GC原理时机以及GC对象；内存泄露场景及解决方法；
gc是java的垃圾回收机制

引用计数法（Reference Counting Collector）:使用计数器来区分存活对象和不再使用的对象。一般来说，堆中的每个对象对应一个引用计数器。当每一次创建一个对象并赋给一个变量时，引用计数器置为1。当对象被赋给任意变量时，引用计数器每次加1当对象出了作用域后(该对象丢弃不再使用)，引用计数器减1，一旦引用计数器为0，对象就满足了垃圾收集的条件。

标记算法（Tracing Collector):使用了根集的概念，基于tracing算法的垃圾收集器从根集开始扫描，识别出哪些对象可达，哪些对象不可达，并用某种方式标记可达对象。

整理算法（Compacting Collecotr）：该算法会将所有的对象移到堆的一端。能解决堆碎片的问题。

复制算法：将内存分为两个区域（from space 和 to space）。所有的对象都分配到from space。清理时先将所有标为活动对象copy到to space，然后清除from space空间。然后互换from space和to apce的身份，每次清理都重复上述过程。
gc收集器：

serial收集器：单线程，工作时必须暂停其他工作线程，多用于client机器上，使用复制算法。

ParNew收集器：serial的多线程版本，server模式下jvm首选的新生代收集器。复制算法。

Parallel Scavenge收集器：可控制吞吐量的收集器，吞吐量指有效运行时间。复制算法。

Serial Old收集器：serial的老年代版本，使用整理算法。

Parallel Old收集器：Parallel Scavenge收集器的老版本，多线程，标记整理。

CMS收集器：整理算法。最短回收停顿时间，缺点是产生碎片。

GI收集器：基本思想是化整为零，将堆分为多个Region，优先回收价值最大的Region。并行并发，分代收集，空间整合。整理算法。

6、四大组件及生命周期；ContentProvider的权限管理(读写分离，权限控制-精确到表级，URL控制)；Activity的四种启动模式对比；Activity状态保存于恢复；

### 7、什么是AIDL 以及如何使用；
>AIDL是Android Interface Definition Language的简写，即Android接口定义语言。
当一个应用想要访问另一个应用的数据或调用其方法，就要用到Android系统提供的IPC机制。而AIDL就是Android实现IPC机制的方式之一。
AIDL是使用bind机制来工作。
AIDL:android interface definition language的缩写。
AIDL是用来实现进程间通信的，可以帮我们实现发布以及调用远程服务。
使用：
1）服务端：创建一个Service用来监听客户端的连接请求，然后创建一个AIDL文件，将服务端暴露给客户端的接口在这个文件中声明，最后在Service中实现这个AIDL接口。
2）客户端：首先绑定服务端的Service，绑定成功后将服务端返回的Binder对象转成AIDL接口所属的类型，接着就可以调用AIDL中的方法。

### 8、请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系；
>简单的说，Handler 获取当前线程中的 Looper 对象，Looper 用来从存放 Message 的MessageQueue 中取出 Message，再交由 Handler 进行 Message 的分发和处理。

9、Fragment生命周期；Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用，如果在Adapter中使用应该如何解耦；

10、AsyncTask原理及不足；intentService原理；

### 11、Activity 怎么和Service 绑定，怎么在Activity 中启动自己对应的Service；
>Activity 通过 bindService(Intent service, ServiceConnection conn, int flags)跟 Service 进行绑定，当绑定成功的时候 Service 会将代理对象通过回调的形式传给 conn，这样我们就拿到了 Service 提供的服务代理对象。
在 Activity 中可以通过 startService 和 bindService 方法启动 Service。

>一般情况下如果想获取 Service 的服务对象那么肯定需要通过bindService（）方法，比如音乐播放器，第三方支付等。如果仅仅只是为了开启一个后台任务那么可以使用 startService（）方法。


### 12、请描述一下Service 的生命周期；
>启动Service时可调用startService和bindService()方法来启动，用这两种方法启动的Service的生命周期是不同的。

启动服务的生命周期 onCreate->onStartCommand()->onDestroy()
绑定服务的生命周期 onCreate()->onBind()->onUnBind()->onDestroy()
途径一：
调用Context.startService()启动Service，调用Context.stopService()或Service.stopSelf()或Service.stopSelfResult()关闭Service的调用。
生命周期顺序为：onCreate()->onStart()->onDestroy()

途径二：
调用Context.bindService()进行初始化绑定，使用Context.unbindService()取消绑定，由于Service和Context是绑定关系，当Context退出或被销毁时，Service也会相应退出。
生命周期顺序为：onCreate->onBind(只一次，不可多次绑定)->onUnbind->onDestroy()

BroadcastReceiver只能通过startService启动Service，因为广播本身生命周期很短，bind的话没有意义,通过bindService创建的服务（但仍然通过bindService得到了服务对象），就可能unbindService后还在运行，否则应该是结束掉了。

13、AstncTask+HttpClient与AsyncHttpClient有什么区别；

### 14、如何保证一个后台服务不被杀死；比较省电的方式是什么；
Android中通过Service实现后台任务。
* 方法一：
通过将Service绑定到Notification，成为一个前提服务，可以提高存活率
在Service中创建一个Notification，再调用Service.startForeground(int id,Notification notification)方法运行在前台即可。这个方式使用360等如阿健管家可以杀死。

* 方法二：
通过定时警报来不断启动Service，这样就算Service被杀死，也能再启动。同时也可以监听网络切换，开锁屏等广播启动Service。
参考：
Intent intent = new Intent（mContext，MyService.class）；
PendingIntent sender = PendingIntent.getService(mContext,0,intent,0);
AlarmManager alarm = (AlarmManager)getSystemService(ALARM_SERVICE);
alarm.setRepeating(AlarmManager.RTC_WAKEUP,System.currentTimeMillis(),5*10000,sender);
这种方式不断启动的逻辑处理起来很麻烦。

* 方法三：
通过jni调用c，在c语音中启动一个进程fork（）。 可以保证360等手机管家不会清理。但是带来了jni交互，稍微有点麻烦。
  
[android如何保证service不被杀死](http://www.voidcn.com/article/p-vaqywkec-go.html)

15、如何通过广播拦截和abort一条短信；广播是否可以请求网络；广播引起anr的时间限制；

16、进程间通信，AIDL；

### 17、事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
**区别：**
>onTouch方法优先级比onTouchEvent高，会先触发。假如onTouch方法返回false，会接着触发onTouchEvent，反之onTouchEvent方法不会被调用。内置诸如click事件的实现等等都基于onTouchEvent，假如onTouch返回true，这些事件将不会被触发。

**使用：**
 * 1、onTouch()方法：
> onTouch方式是View的OnTouchListener接口中定义的方法。当一个View绑定了OnTouchListener后，当有Touch事件触发时，就会调用onTouch方法。
 * 2、onTouchEvent()方法：
> onTouchEvent方法时重载的Activity的方法 重写了Acitivity的onTouchEvent方法后，当屏幕有Touch事件时，此方法就会被调用。

18、说说ContentProvider、ContentResolver、ContentObserver 之间的关系；

### 19、请介绍下ContentProvider 是如何实现数据共享的；
Android提供了ContentProvider，一个程序可以通过实现一个ContentProvider的抽象接口将自己的数据完全暴露出去，而且ContentProvicer是以类似数据库中表的方式将数据暴露。也就是说ContentProvider就像一个“数据库”。那么外界获取其提供的数据，也就应该与从数据库中获取数据的操作基本一样，只不过是采用URI来表示外界需要访问的“数据库”。外部访问通过ContentResolver去访问并操作这些被暴露的数据。

### 20、Handler机制及底层实现；
Handler包括四个角色：
* Handler：负责发送消息处理消息。
* Message：消息实体对象，handler通过sendMessage将实体放到消息队列中。
* MessageQueQue:存放消息的队列。
* Looper：消息轮询器，不停的从消息队列中取出消息交给handler处理。
  
在主线程创建Handler，在需要发送消息的地方创建一个Message，通过handler发送。这个消息存到MessageQueQue中，然后Looper会将这个消息取出交给handler处理。

### 21、Binder机制及底层实现；
Binder包含四个角色：
* Server 服务器
* Client 客户终端 ，获得实名Binder的引用。Server向ServiceManger注册了Binder实体及名字后，Client就可以通过名字获得该Binder的引用。例如我们申请获得名字叫张三的Binder的引用，ServiceManager收到这个连接请求，从请求数据包里获得Binder的名字。再找到该名字对应的条目，从条目中取出Binder的引用。将该引用作为回复发送给发起请求的Client。
* ServiceManager 域名服务器（DNS），负责将字符形式的Binder名字转化成Client中对该Binder的应用，使得Client能通过Binder名字获得Server中Binder实体的引用。
* Binder驱动 可以理解为路由器。Binder驱动负责进程之间Binder通信的建立，Binder在进程间的传递。Binder使用Client-Server通信方式，安全性好，简单高效。再加上其面向对象的设计思想，独特的接收缓存管理和线程池管理方式，成为Android进程间通信的中流砥柱。

### 22、ListView 中图片错位的问题是如何产生的；
错位原理： 如果我们只是简单的显示数据，没有convertView的复用和异步操作，就不会产生图片错位。重用convertView但没有异步操作也不会有错位现象。

例如我们的listView中刚好显示7个item，当向下滑动时，显示出item8，而item8是重用的item1，如果此时异步网络请求item8的图片，比item1的图片慢，那么item8就会显示item1的图片。当item8下载完成，此时用户向上滑显示item1时，又复用的item8的image。这样就导致的图片错位。

解决方法： 对imageview设置tag，并预设一张图片。

### 23、在manifest 和代码中如何注册和使用BroadcastReceiver；
```
<receiver android:name="包名.自己扩展的广播接收者名">
   <intent-filter>
      <action android:name="android.provider.Telephony.SMS_RECEIVED"/>
   </intent-filter>
</receiver>
```

24、说说Activity、Intent、Service 是什么关系；

### 25、ApplicationContext和ActivityContext的区别；
>这是两种不同的context，也是最常见的两种.第一种中context的生命周期与Application的生命周期相关的，context随着Application的销毁而销毁，伴随application的一生，与activity的生命周期无关.第二种中的context跟Activity的生命周期是相关的，但是对一个Application来说，Activity可以销毁几次，那么属于Activity的context就会销毁多次.至于用哪种context，得看应用场景，个人感觉用Activity的context好一点，不过也有的时候必须使用Application的context.application context

26、一张Bitmap所占内存以及内存占用的计算；

### 27、Serializable 和Parcelable 的区别；
>在Android上应该尽量采用Parcelable，它效率更高。
Parcelabe代码比Serializable多一些。
Parcelabe比Serializable速度高十倍以上。
Serializable只需要对某个类以及它的属性实现Serializable接口即可，无需实现方法。缺点是使用的反射，序列化的过程较慢，这种机制会在序列化的时候创建许多的临时对象。容易触发GC。
Parcable方法实现的原理是将一根完整的对象进行分解，而分解后的每一部分都是Intent所支持的数据类型，这样也就实现传递对象的功能。

28、请描述一下BroadcastReceiver；

29、请描述一下Android 的事件分发机制；

30、请介绍一下NDK；

31、什么是NDK库，如何在jni中注册native函数，有几种注册方式；

32、AsyncTask 如何使用；

33、对于应用更新这块是如何做的？(灰度，强制更新，分区域更新)；

34、混合开发，RN，weex，H5，小程序(做Android的了解一些前端js等还是很有好处的)；

### 35、什么情况下会导致内存泄露；
根本原因：长生命周期的对象持有短生命周期的对象。短周期对象就无法及时释放。
* 静态集合类引起内存泄露
* remove 方法无法删除set集  Objects.hash(firstName, lastName);
* observer 我们在使用监听器的时候，往往是addxxxlistener，但是当我们不需要的时候，忘记removexxxlistener，就容易内存leak。广播没有unregisterrecevier
* 各种数据链接没有关闭，数据库contentprovider，io，sokect等。cursor
* 内部类
* 单例
  单例 是一个全局的静态对象，当持有某个复制的类A是，A无法被释放，内存leak。

### 36、如何对Android 应用进行性能分析以及优化；
>android 性能主要之响应速度 和UI刷新速度。
* 首先从函数的耗时来说，有一个工具TraceView 这是androidsdk自带的工作，用于测量函数耗时的。

* UI布局的分析，可以有2块，一块就是Hierarchy Viewer 可以看到View的布局层次，以及每个View刷新加载的时间。这样可以很快定位到那块layout & View 耗时最长。

* 还有就是通过自定义View来减少view的层次。

37、说一款你认为当前比较火的应用并设计(直播APP)；

### 38、OOM的避免异常及解决方法；
当前占用的内存加上app申请的内存资源超过了Dvlvik虚拟机的最大内存限制导致抛出Out of memory异常。


### 39、屏幕适配的处理技巧都有哪些；
屏幕适配的方式：xxxdpi， wrap_content,match_parent. 获取屏幕大小，做处理。

* dp来适配屏幕，sp来确定字体大小

* drawable-xxdpi, values-1280*1920等 这些就是资源的适配。

* wrap_content,match_parent, 这些是view的自适应

* weight，这是权重的适配。

40、两个Activity 之间跳转时必然会执行的是哪几个方法？

40、Okhttp原理

41、Rxjava用法和原理

42，热更新技术有哪些，知道的原理！

43、Activity启动流程

44、Android内存管理

45、Android权限管理

46、将一下7.0的新特性

47、说下你你们项目的架构

48、组件化的有点和具体实施方案

49、内存泄露检测方法

50、Http协议，SSL握手机制。

## Java基础

1、集合类以及集合框架；HashMap与HashTable实现原理，线程安全性，hash冲突及处理算法；ConcurrentHashMap；

2、进程和线程的区别；

3、Java的并发、多线程、线程模型；

4、什么是线程池，如何使用?
答：线程池就是事先将多个线程对象放到一个容器中，当使用的时候就不用new 线程而是直接去池中拿线程即可，节省了开辟子线程的时间，提高的代码执行效率。

5、数据一致性如何保证；Synchronized关键字，类锁，方法锁，重入锁；

6、Java中实现多态的机制是什么；

7、如何将一个Java对象序列化到文件里；

8、说说你对Java反射的理解；
答：Java 中的反射首先是能够获取到Java 中要反射类的字节码， 获取字节码有三种方法，
* (1).Class.forName(className)
* (2).类名.class
* (3).this.getClass()。
  
然后将字节码中的方法，变量，构造函数等映射成相应的Method、Filed、Constructor 等类，这些类提供了丰富的方法可以被我们所使用。

9、同步的方法；多进程开发以及多进程应用场景；

10、在Java中wait和seelp方法的不同；
答：最大的不同是在等待时wait 会释放锁，而sleep 一直持有锁。wait 通常被用于线程间交互，sleep 通常被用于暂停执行。

11、synchronized 和volatile 关键字的作用；
答：
* 1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
* 2）禁止进行指令重排序。

12、volatile 本质是在告诉jvm 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized 则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
   
   * (1).volatile 仅能使用在变量级别；synchronized 则可以使用在变量、方法、和类级别的
   * (2).volatile 仅能实现变量的修改可见性，并不能保证原子性；synchronized 则可以保证变量的修改可见性和原子性
   * (3).volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。
   * (4).volatile 标记的变量不会被编译器优化；synchronized 标记的变量可以被编译器优化

13、服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达；

14、ThreadLocal原理，实现及如何保证Local属性；

15、String StringBuilder StringBuffer对比；

16、你所知道的设计模式有哪些；
答：
Java 中一般认为有23 种设计模式，我们不需要所有的都会，但是其中常用的几种设计模式应该去掌握。下面列出了所有的设计模式。需要掌握的设计模式我单独列出来了，当然能掌握的越多越好。
总体来说设计模式分为三大类：
创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

17、Java如何调用c、c++语言；

18、接口与回调；回调的原理；写一个回调demo；

19、泛型原理，举例说明；解析与分派；

20、抽象类与接口的区别；应用场景；抽象类是否可以没有方法和属性；

21、静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？

22、修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法；

23、说说你对泛型的了解；

24、Java的异常体系；

25、如何控制某个方法允许并发访问线程的个数；

26、动态代理的区别，什么场景使用；

27、Dex加载过程和优化方式；

28、Jvm和Gc机制；

29、常用的设计模式。

30、Java的引用有哪些

> 这4种引用的级别由高到低依次为：
强引用 > 软引用 > 弱引用 > 虚引用
> * **强引用（StrongReference）**:
当一个对象具有强引用时，垃圾回收器不会回收它，即使虚拟机抛出OutOfMemoryError错误，也不会随意回收强引用的对象来释放内存。
> * **软引用（SoftReference）**: 软引用描述那些不是必需的但仍然有用的对象。只要内存空间足够，垃圾回收器就不会回收它；如果内存空间不足，就会回收它。软引用通常用于实现高速缓存，以便在内存不足时释放缓存中的对象。
> * **弱引用（WeakReference）**:
弱引用用于描述那些不会阻止对象被垃圾回收的对象。当JVM进行垃圾回收时，无论内存是否充足，都会回收弱引用指向的对象。
> * **虚引用（PhantomReference）**:
如果一个对象与虚引用关联，则跟没有引用与之关联一样，在任何时候都可能被垃圾回收器回收。虚引用主要用来跟踪对象被垃圾回收的活动。当对象被回收时，虚引用会被放入引用队列中，以便应用程序可以了解到对象何时被回收。

## 数据结构与算法

1、堆和栈在内存中的区别是什么(数据结构方面以及实际实现方面)；

2、最快的排序算法是哪个？给阿里2万多名员工按年龄排序应该选择哪个算法？堆和树的区别；写出快排代码；链表逆序代码；

3、求1000以内的水仙花数以及40亿以内的水仙花数；

4、子串包含问题(KMP 算法)写代码实现；

5、万亿级别的两个URL文件A和B，如何求出A和B的差集C,(Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)

6蚁群算法与蒙特卡洛算法；

7、写出你所知道的排序算法及时空复杂度，稳定性；

8、百度POI中如何试下查找最近的商家功能(坐标镜像+R树)。

9、遍历二叉树

10、自己集合实现一个队列

11、自己实现线程安全类

12、快速排序和冒泡的排序，怎么转换一下。

## 其它

1、死锁的四个必要条件；

2、常见编码方式；utf-8编码中的中文占几个字节；int型几个字节；

3、实现一个Json解析器(可以通过正则提高速度)；

4、MVC MVP MVVM; 常见的设计模式；写出观察者模式的代码；

5、TCP的3次握手和四次挥手；TCP与UDP的区别；

6、HTTP协议；HTTP1.0与2.0的区别；HTTP报文结构；

7、HTTP与HTTPS的区别以及如何实现安全性；

8、都使用过哪些框架、平台；

9、都使用过哪些自定义控件；

10、介绍你做过的哪些项目；

## 非技术问题汇总

1、研究比较深入的领域有哪些；
2、对业内信息的关注渠道有哪些；
3、最近都读哪些书；
4、自己最擅长的技术点，最感兴趣的技术领域和技术点；
5、项目中用了哪些开源库，如何避免因为引入开源库而导致的安全性和稳定性问题；
6、实习过程中做了什么，有什么产出；
7、5枚硬币，2正3反如何划分为两堆然后通过翻转让两堆中正面向上的硬8币和反面向上的硬币个数相同；
8、时针走一圈，时针分针重合几次；
9、N * N的方格纸,里面有多少个正方形；
10、现在下载速度很慢,试从网络协议的角度分析原因,并优化(网络的5层都可以涉及)。

##  HR问题汇总
1、您在前一家公司的离职原因是什么？
2、讲一件你印象最深的一件事情；
3、介绍一个你影响最深的项目；
4、介绍你最热爱最擅长的专业领域；
5、公司实习最大的收获是什么；
6、与上级意见不一致时，你将怎么办；
7、自己的优点和缺点是什么？并举例说明？
8、你的学习方法是什么样的？实习过程中如何学习？实习项目中遇到的最9、大困难是什么以及如何解决的；
10、说一件最能证明你能力的事情；
11、针对你你申请的这个职位，你认为你还欠缺什么；
12、如果通过这次面试我们单位录用了你，但工作一段时间却发现你根本13、不适合这个职位，你怎么办；
14、项目中遇到最大的困难是什么？如何解决的；
15、你的职业规划以及个人目标；未来发展路线及求职定位；
16、如果你在这次面试中没有被录用，你怎么打算；
17、评价下自己，评价下自己的技术水平，个人代码量如何；
18、通过哪些渠道了解的招聘信息，其他同学都投了哪些公司；
19、业余都有哪些爱好；
20、你做过的哪件事最令自己感到骄傲；
21、假如你晚上要去送一个出国的同学去机场，可单位临时有事非你办不可，你怎么办；
22、就你申请的这个职位，你认为你还欠缺什么；
23、当前的offer状况；如果BATH都给了offer该如何选；
24、你对一份工作更看重哪些方面？平台，技术，氛围，城市，money；
25、理想薪资范围；杭州岗和北京岗选哪个；
26、理想中的工作环境是什么；
27、谈谈你对跳槽的看法；
28、说说你对行业、技术发展趋势的看法；
29、实习过程中周围同事/同学有哪些值得学习的地方；
30、家人对你的工作期望及自己的工作期望；
31、如果你的工作出现失误，给本公司造成经济损失，你认为该怎么办；
32、若上司在公开会议上误会你了，该如何解决；
32、是否可以实习，可以实习多久；
33、在五年的时间内，你的职业规划；
34、你看中公司的什么？或者公司的那些方面最吸引你。
