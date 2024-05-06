---
layout: post
title: ImageView 中的 ScaleType 不起作用
toc: true
reward: true
date: 2023-02-28 15:03:19
tags: [bug]
categories: [Android]
---
我正在尝试使用 ViewPager展示Banner图片库。 viewpager 和图像检索一切正常。但是当我将位图放在 ImageView 中时，图像不会被拉伸以填充 ImageView 的大小。

就是在设置imageview的scaleType属性的时候 无论怎么设置图片没有变化，后来猜想是图片的背景填充和src引用的区别 说白了就是background和src的关系，xml中或者是代码中设置图片的填充形式为background的话，那么imageview的scaletype是没有效果的。

**设置了background了后，那么imageview的scaletype是没有效果的**

