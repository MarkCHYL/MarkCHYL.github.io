---
layout: post
title: Activity的生命周期
date: 2018-12-17 09:31:39
toc: true
reward: true
tags: [基础理论]
categories: [Android]
---
![image](http://hi.csdn.net/attachment/201109/1/0_1314838777He6C.gif)
#### 1.启动Activity：系统会先调用onCreate方法，这是生命周期第一个方法，然后调用onStart方法，最后调用onResume，Activity进入运行状态。
<!--more-->
onCreate方法：一般做一些初始化工作，比如setContentView去加载布局资源，初始化Activity所需的数据。

onStart方法：表示Activity正在启动，已经可见，但是无法和用户交互。
/Users/mark/Downloads/Activity的生命周期.md
onResume方法：Activity已经可见并且开始活动，已经出现在前台。

#### 2.当前Activity被其他Activity覆盖其上或被锁屏：

（可以理解为没有完全遮挡界面的）

系统会调用onPause方法，暂停当前Activity的执行。

#### 3.当前Activity由被覆盖状态回到前台或解锁屏：

系统会调用onResume方法，再次进入运行状态。

#### 4.当前Activity转到新的Activity界面或按Home键回到主屏，自身退居后台：

系统会先调用onPause方法，然后调用onStop方法，进入停滞状态。

#### 5.用户后退回到此Activity：

系统会先调用onRestart方法，

然后调用onStart方法，

最后调用onResume方法，

再次进入运行状态。

#### 6.用户退出当前Activity：系统先调用onPause方法，然后调用onStop方法，最后调用onDestory方法，结束当前Activity。

但是知道这些还不够，我们必须亲自试验一下才能深刻体会，融会贯通。

***
Activity四种启动模式的区别：standard    singleTop  singleTask   singleInstance

standard：每次激活Activity时(startActivity)，都创建Activity实例，并放入任务栈； 

singleTop：如果某个Activity自己激活自己并且Activity处于栈顶，则不需要创 建，其余情况都要创建Activity实例；
 
singleTask：如果要激活的那个Activity在任务栈中存在该实例，则不需要创建，只需要把 此Activity放入栈顶，即把该Activity以上的Activity实例都pop，并调用其onNewIntent；
 
singleInstance：应用1的任务栈中创建了MainActivity实例，如果应用2也要激活 MainActivity，则不需要创建，两应用共享该Activity实例。
***
onSaveInstanceState的调用遵循一个重要原则，即当系统“未经你许可”时销毁了你的 activity，则onSaveInstanceState会被系统调用，这是系统的责任，因为它必须要提供一 个机会让你保存的数据。
至于onRestoreInstanceState方法，需要注意的是， onSaveInstanceState方法和onRestoreInstanceState方法“不一定”是成对的被调用 的。 

onRestoreInstanceState被调用的前提是，activity A“确实”被系统销毁了，而如果仅仅 是停留在有这种可能性的情况下，则该方法不会被调用，例如，当正在显示activity A的时 候，用户按下HOME键回到主界面，然后用户紧接着又返回到activity A，这种情况下 activity A一般不会因为内存的原因被系统销毁
故activity A的onRestoreInstanceState方 法不会被执行。

 另外，onRestoreInstanceState的bundle参数也会传递到onCreate方法中，你也可以选择 在onCreate方法中做数据还原。

切换横竖屏的生命周期：


1、不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，
切横屏时会执行一次，切竖屏时会执行两次

2、设置Activity的android:configChanges="orientation"时，切屏还是会重新调
用各个生命周期，切横、竖屏时只会执行一次

3、设置Activity的android:configChanges="orientation|keyboardHidden"时，
切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法




#### Activity运行时按下HOME键(跟被完全覆盖是一样的)：
---
--> onPause--> onPauseonSaveInstanceState--> onStop  --> onRestart-->onStart--->onResume
---
#### Activity未被完全覆盖只是失去焦点：onPause--->onResume（我没复现过这个，待考证）

#### A跳转到B，然后按 back回退到A，A的生命周期：
---
-->onCreate -->onStart -->onResume -->onPause -->onSaveInstanceState -->onStop 然后回退-->onRestart -->onStart -->onResume 
---
