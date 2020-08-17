---
layout: post
title: Android 搜索 把软键盘上的回车键改为搜索
date: 2019-09-25 15:33:45
toc: true
reward: true
tags: [Android]
---

### 前言：
> 项目中需要自定义一个搜做的功能，但是我通过更具UI的显示，完全没法使用V7包下的SearchView控件，于是我使用EditView自己写的。
![](https://thumbnail0.baidupcs.com/thumbnail/cede3f055d8aeb66477c40eb3af2066a?fid=3761439320-250528-957214977420267&time=1569398400&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-T%2FjYZusknxVB1gKzQcIJFFErJ%2FM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=443423899171394619&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)

<!--more-->
### 具体实现如下：
* 修改Editview属性：
  
  `android:imeOptions="actionSearch"   `

  在该Editview获得焦点的时候将“回车”键改为“搜索”

  android:singleLine="true"     

  不然回车【搜索】会换行。

* EditView编写监听按下控件监听，监听确认键的按下
```
etSearchstr.setOnKeyListener(new View.OnKeyListener() {
            @Override
            public boolean onKey(View v, int keyCode, KeyEvent event) {
                if (keyCode == KeyEvent.KEYCODE_ENTER) {
                    // 先隐藏键盘
                    ((InputMethodManager) getSystemService(INPUT_METHOD_SERVICE)).hideSoftInputFromWindow(GoodsListActivity.this.getCurrentFocus()
                            .getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);
                    //进行搜索操作的方法，在该方法中可以加入mEditSearchUser的非空判断
                    refresh.autoRefresh();
                }
                return false;
            }
    } 
```
