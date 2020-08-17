---
layout: post
title: 自定义Toat及其弹出时间控制
toc: true
reward: true
date: 2020-07-21 16:47:53
tags: [Android]
---
效果就是实现Toast布局自定义，弹出的事件进行控制
### 一、先自定义布局
```
  Toast toast = Toast.makeText(mContext, str, Toast.   LENGTH_SHORT);
  View view = LayoutInflater.from(mContext).inflate(R.layout.item_toast, null);
  TextView textView = view.findViewById(R.id.str_toast);
        textView.setText(str);
        toast.setView(view);
        toast.setGravity(Gravity.CENTER, 0, 0);
  Utils.showTimeToast(toast,500);
```
### 二、自定义弹出时间
```
 /**
     * Toast自定义时间
     * Toast对象时间需要为Toast.LENGH_LONG
     */
    public static void showTimeToast(final Toast toast, final int time) {
        final Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                toast.show();
            }
        }, 0, 3000);
        new Timer().schedule(new TimerTask() {
            @Override
            public void run() {
                toast.cancel();
                timer.cancel();
            }
        }, time);
    }
```
就是那么简单，利用Timer和schedule实现事件监听，控制toast的弹出时间。

