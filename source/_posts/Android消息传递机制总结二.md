---
layout: post
title: Android消息传递机制总结(二)
toc: true
reward: true
date: 2020-10-10 10:38:54
tags: [消息通信]
categories: [Android]
---
组件间通信 ——— BroadcastReceiver，接口回调等。
### 2.组件间通信
BroadcastReceiver广播就不再介绍， 广播传递本身是有安全隐患的，需要设置权限，每一个Activity都要定义、注册，解注册广播无形中加大了工作量和维护成本。已经不适应用在组件间通信。
<!-- more -->
接口回调： 和观察者模式大致一样。
实例：
```

public class DataSynManager {
    //监听集合
    private LinkedList<IDataSynListener> autoListeners = new LinkedList<>();
    //单例引用
    private static DataSynManager mInstance;

    /**
     * 获取单例引用
     *
     * @return
     */
    public static DataSynManager getInstance() {
        if (mInstance == null) {
            synchronized (DataSynManager.class) {
                if (mInstance == null) {
                    mInstance = new DataSynManager();
                }
            }
        }
        return mInstance;
    }

    /**
     * 添加同步数据监听
     *
     * @param listener
     */
    public void registerDataSynListener(IDataSynListener listener) {
        if (autoListeners == null) {
            autoListeners = new LinkedList<>();
        }
        if (!autoListeners.contains(listener)) {
            autoListeners.add(listener);
        }
    }

    /**
     * 移除同步数据监听
     *
     * @param listener
     */
    public void unRegisterSynListener(IDataSynListener listener) {
        if (autoListeners == null) {
            return;
        }
        if (autoListeners.contains(listener)) {
            autoListeners.remove(listener);
        }
    }

    /**
     * 执行数据同步
     *
     * @param count
     */
    public void doDataSyn(final int count) {
        if (autoListeners == null) {
            autoListeners = new LinkedList<>();
        }
        new Handler().post(new Runnable() {
            @Override
            public void run() {
                for (IDataSynListener dataSynListener :
                        autoListeners) {
                    dataSynListener.onDataSyn(count);
                }
            }
        });
    }

    /**
     * 清除所有监听者
     */
    public void release() {
        if (autoListeners != null) {
            autoListeners.clear();
            autoListeners = null;
        }
    }

    public interface IDataSynListener {
        void onDataSyn(int count);
    }
}

```
使用：
```
//添加监听
DataSynManager.getInstance().registerDataSynListener(dataSynListener);
//移除监听
DataSynManager.getInstance().unRegisterDataSynListener(dataSynListener//个监听);


DataSynManager.IDataSynListener dataSynListener=new DataSynManager.IDataSynListener() {
        @Override
        public void onDataSyn(int count) {
            //接下来执行同步操作
        }
      }

   };
//发送事件
DataSynManager.getInstance().doDa(5);
```
[3. 第三方通信 ——— EventBus，rxBus](http://markchyl.cn/2020/10/10/Android%E6%B6%88%E6%81%AF%E4%BC%A0%E9%80%92%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93%E4%B8%89/)