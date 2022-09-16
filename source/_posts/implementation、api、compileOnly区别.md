---
layout: post
title: implementation、api、compileOnly区别
toc: true
reward: true
date: 2022-09-16 22:51:03
tags: [Android]
categories: [Android]
---
### 2.x和3.x版本依赖方式比较
|2.x|3.x|
|-|-|
|compile|implementation、api|
|provided|compile only|
|apk|runtime only|
|api|api|

### 替代关系：
* compile依赖关系已被弃⽤，被implementation和api替代;
* provided被compile only替代;
* apk被runtime only替代;
* api：跟2.x版本的 compile完全相同。
  
<!-- more -->
### implementation和api区别：
> implementation：只能在内部使⽤此模块，⽐如我在⼀个libiary中使⽤implementation依赖了gson库，然后我的主项⽬依赖了libiary，那么，我的主项⽬就⽆法访问gson库中的⽅法。这样的好处是编译速度会加快，推荐使⽤implementation的⽅式去依赖，如果你需要提供给外部访问，那么就使⽤api依赖即可

### provided（compileOnly）作⽤：
> 只在编译时有效，不会参与打包可以在⾃⼰的moudle中使⽤该⽅式依赖⼀些⽐如com.android.support，gson这些使⽤者常⽤的库，避免冲突。

### apk（runtimeOnly）作⽤：
> 只在⽣成apk的时候参与打包，编译时不会参与，很少⽤。

### testCompile（testImplementation）作⽤：
 > testCompile 只在单元测试代码的编译以及最终打包测试apk时有效。

### debugCompile（debugImplementation）作⽤：
> debugCompile 只在debug模式的编译和最终的debug apk打包时有效

### releaseCompile（releaseImplementation）作⽤：
> Release compile 仅仅针对Release 模式的编译和最终的Release apk打包。
