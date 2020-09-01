---
layout: post
title: 占位符 Broccoli 的使用
date: 2019-01-18 15:20:12
toc: true
reward: true
tags: [Android,自定义控件]
categories: [Android]
---
![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/Broccoli.png)

在预加载时显示view的占位符效果

### 截图
#### 默认效果:

![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/screenshot1.gif)

 <!--more-->
#### 自定义动画效果:
![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/screenshot2.gif)

#### 在RecyclerView使用
![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/screenshot3.gif)


#### 钉钉考勤
![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/screenshot4.png) 

![Broccoli](https://github.com/samlss/Broccoli/raw/master/screenshots/screenshot5.png)
------
### 依赖

#### Gradle
将其添加到你的module的build.gradle中： 

  ```java
  dependencies {
      implementation 'me.samlss:broccoli:1.0.0'
  }
  ```

#### Maven
```java
<dependency>
  <groupId>me.samlss</groupId>
  <artifactId>broccoli</artifactId>
  <version>1.0.0</version>
  <type>pom</type>
</dependency>
```

### 代码使用

```java
Broccoli broccoli = new Broccoli();

//添加默认的占位符
broccoli.addPlaceholders('activity', 'view_id', 'view_id'); 

or 
//添加默认的占位符
broccoli.addPlaceholders('view1', 'view2', 'view3'); 

or 

//添加自定义的占位符
broccoli.addPlaceholder(new PlaceholderParameter.Builder()
                        .setView('view')
                        .setAnimation('scaleAnimation');
                        .setDrawable(DrawableUtils.createRectangleDrawable(placeHolderColor, 0))
                        .build()); 

or
//添加带渐变动画的占位符
broccoli.addPlaceholder(new PlaceholderParameter.Builder()
                        .setView('view')
                        .setDrawable(new BroccoliGradientDrawable(Color.parseColor("#DDDDDD"),
                            Color.parseColor("#CCCCCC"), 0, 1000, new LinearInterpolator())
                        .build()); 
broccoli.show();
```

[源码demo地址](https://github.com/MarkCHYL/Mark_Broccoli_demo)