---
layout: post
title: Android消息传递机制总结(四)
toc: true
reward: false
date: 2020-10-12 09:04:25
tags: [消息通信,AIDL]
categories: [Android]
---
进程间通信 ——— Content Provider ，Broadcast ，AIDL等。
### 4.进程间通信
#### Content Provider
Android应用程序可以使用文件或SqlLite数据库来存储数据。Content Provider提供了一种在多个应用程序之间数据共享的方式（跨进程共享数据）。应用程序可以利用Content Provider完成下面的工作
1. 查询数据
2. 修改数据
3. 添加数据
4. 删除数据
<!-- more -->
虽然Content Provider也可以在同一个应用程序中被访问，但这么做并没有什么意义。Content Provider存在的目的向其他应用程序共享数据和允许其他应用程序对数据进行增、删、改操作。
Android系统本身提供了很多Content Provider，例如，音频、视频、联系人信息等等。我们可以通过这些Content Provider获得相关信息的列表。这些列表数据将以Cursor对象返回。因此，从Content Provider返回的数据是二维表的形式。
#### 广播（Broadcast）
广播是一种被动跨进程通讯的方式。当某个程序向系统发送广播时，其他的应用程序只能被动地接收广播数据。这就象电台进行广播一样，听众只能被动地收听，而不能主动与电台进行沟通。
在应用程序中发送广播比较简单。只需要调用sendBroadcast方法即可。该方法需要一个Intent对象。通过Intent对象可以发送需要广播的数据。
#### AIDL Service
这是我个人比较推崇的方式，因为它相比Broadcast而言，虽然实现上稍微麻烦了一点，但是它的优势就是不会像广播那样在手机中的广播较多时会有明显的时延，甚至有广播发送不成功的情况出现。
注意普通的Service并不能实现跨进程操作，实际上普通的Service和它所在的应用处于同一个进程中，而且它也不会专门开一条新的线程，因此如果在普通的Service中实现在耗时的任务，需要新开线程。
要实现跨进程通信，需要借助AIDL(Android Interface Definition Language)。Android中的跨进程服务其实是采用C/S的架构，因而AIDL的目的就是实现通信接口。
[AIDL具体使用可参考](http://www.jianshu.com/p/d1fac6ccee98)

### 什么是AIDL？
AIDL:Android Interface Definition Language,即Android接口定义语言。

Android系统中的进程之间不能共享内存，因此，需要提供一些机制在不同进程之间进行数据通信。为了使其他的应用程序也可以访问本应用程序提供的服务，Android系统采用了远程过程调用（Remote Procedure Call，RPC）方式来实现。与很多其他的基于RPC的解决方案一样，Android使用一种接口定义语言（Interface Definition Language，IDL）来公开服务的接口。我们知道4个Android应用程序组件中的3个（Activity、BroadcastReceiver和ContentProvider）都可以进行跨进程访问，另外一个Android应用程序组件Service同样可以。因此，可以将这种可以跨进程访问的服务称为AIDL（Android Interface Definition Language）服务。
### AIDL的使用
因为是两个APP交互么，所以当然要两个APP啦，我们在第一个工程目录右键
![](https://upload-images.jianshu.io/upload_images/2099385-d68440c5786c6047.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/659/format/webp)
输入名称后，sutido就帮我们创建了一个AIDL文件。
```
// IMyAidlInterface.aidl
package cc.abto.demo;

// Declare any non-default types here with import statements

interface IMyAidlInterface {
    /**
     * Demonstrates some basic types that you can use as parameters
     * and return values in AIDL.
     */
    void basicTypes(int anInt, long aLong, boolean aBoolean, float aFloat,
            double aDouble, String aString);
}
```
上面就是studio帮我生成的aidl文件。basicTypes这个方法可以无视，看注解知道这个方法只是告诉你在AIDL中你可以使用的基本类型（int, long, boolean, float, double, String），因为这里是要跨进程通讯的，所以不是随便你自己定义的一个类型就可以在AIDL使用的，这些后面会说。我们在AIDL文件中定义一个我们要提供给第二个APP使用的接口。
```
interface IMyAidlInterface {
   String getName();
}
```
定义好之后，就可以sycn project一下，然后新建一个service。在service里面创建一个内部类，继承你刚才创建的AIDL的名称里的Stub类,并实现接口方法,在onBind返回内部类的实例。
```
public class MyService extends Service
{

    public MyService()
    {

    }

    @Override
    public IBinder onBind(Intent intent)
    {
        return new MyBinder();
    }

    class MyBinder extends IMyAidlInterface.Stub
    {

        @Override
        public String getName() throws RemoteException
        {
            return "test";
        }
    }
}
```
接下来，将我们的AIDL文件拷贝到第二个项目，然后sycn project一下工程。
![](https://upload-images.jianshu.io/upload_images/2099385-585fbc5fb15906e8.png?imageMogr2/auto-orient/strip|imageView2/2/w/314/format/webp)
**这边的包名要跟第一个项目的一样哦，这之后在Activity中绑定服务。**
```

public class MainActivity extends AppCompatActivity {

    private IMyAidlInterface iMyAidlInterface;
    ServiceConnection conn;
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Intent intent = new Intent();
        intent.setAction("com.mark.aidldemo1");
        intent.setPackage("com.mark.aidldemo1");

        conn = new ServiceConnection() {
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                iMyAidlInterface = IMyAidlInterface.Stub.asInterface(service);
            }

            @Override
            public void onServiceDisconnected(ComponentName name) {

            }
        };
        bindService(intent, conn, BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        unbindService(conn);
        super.onStop();
    }

    public void doTest(View view) {
        try {
            if (iMyAidlInterface!=null) {
                Toast.makeText(this, iMyAidlInterface.getName(), Toast.LENGTH_SHORT).show();
            }
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }

}
```
这边我们通过隐式意图来绑定service，在onServiceConnected方法中通过IMyAidlInterface.Stub.asInterface(service)获取iMyAidlInterface对象，然后在onClick中调用iMyAidlInterface.getName()。
![](https://upload-images.jianshu.io/upload_images/2099385-0bc46eaf6923f712.png?imageMogr2/auto-orient/strip|imageView2/2/w/564/format/webp)

### 自定义类型
如果我要在AIDL中使用自定义的类型，要怎么做呢。首先我们的自定义类型要实现Parcelable接口，下面的代码中创建了一个User类并实现Parcelable接口。这边就不对Parcelable进行介绍了，不熟悉的童鞋自行查找资料，总之我们这边可以借助studio的Show Intention Action（也就是Eclipse中的Quick Fix，默认是alt+enter键）帮我们快速实现Parcelable接口。
![](https://upload-images.jianshu.io/upload_images/2099385-575c252bdc2790f1.png?imageMogr2/auto-orient/strip|imageView2/2/w/615/format/webp)

接下新建一个aidl文件，名称为我们自定义类型的名称，这边是User.aidl。在User.aidl申明我们的自定义类型和它的完整包名，注意这边parcelable是小写的，不是Parcelable接口，一个自定类型需要一个这样同名的AIDL文件。
```
package com.mark.aidldemo1;
parcelable User;
```
然后再在我们的AIDL接口中导入我们的AIDL类型。
```
import com.mark.aidldemo1.User;

// Declare any non-default types here with import statements

interface IMyAidlInterface {
   String getName();
   User getUserName();
}
```
然后定义接口方法，sycn project后就可以在service中做具体实现了。
```
  class MyBinder extends IMyAidlInterface.Stub
    {

       ........

        @Override
        public User getUserName() throws RemoteException {
            return new User("Mark");
        }
    }
```
最后将我们的AIDL文件和自定义类型的java一并拷贝到第二个项目，注意包名都要一样哦

![](https://upload-images.jianshu.io/upload_images/2099385-63e992963f1bd552.png?imageMogr2/auto-orient/strip|imageView2/2/w/246/format/webp)
然后就可以在Activity中使用该自定义类型的AIDL接口了
```
public class MainActivity extends AppCompatActivity
{
    //...
    public void onClick(View view)
    {
        try
        {
            Toast.makeText(MainActivity.this, iMyAidlInterface.getUserName().getName(), Toast.LENGTH_SHORT).show();
        }
        catch (RemoteException e)
        {
            e.printStackTrace();
        }
    }
}
```
AIDL只是Android中众多进程间通讯方式中的一种方式,[我的代码](https://gitee.com/markshow/msg-ipc)

### [下一章长链接推送](http://markchyl.cn/2020/10/19/Android%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93-%E4%BA%94/)