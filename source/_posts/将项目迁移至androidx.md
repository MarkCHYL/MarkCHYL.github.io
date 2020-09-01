---
layout: post
title: 将项目迁移至androidx
toc: true
reward: true
date: 2019-10-14 13:25:50
tags: [Android]
categories: [Android]
---
## 自己的工具：
* Android studio3.4.1
* MacBook
  
## 迁移步骤：
* 在gradle.properties添加如下内容：
```
  /**
*android.useAndroidX=true 表示当前项目启用 androidx
*android.enableJetifier=true 表示将依赖包也迁移到androidx 。如果取值为false,表示不迁移依赖包
*到androidx，但在使用依赖包中的内容时可能会出现问题
*/
android.useAndroidX=true   
android.enableJetifier=true 
```
<!--more-->
* 在AndroidStudio 3.2或更高（因为一个个去改太麻烦，这个版本有一键迁移的功能 Refactor -> Migrate to AndroidX  在执行该操作时会提醒我们是否将当前项目打包备份。如果你提前已经做好了备份，可以忽略；如果没有备份，则先备份。）
![示意图](https://img-blog.csdnimg.cn/20190711092020175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNzcxMjU4,size_16,color_FFFFFF,t_70)

*gradle版本至少为3.2.0以上，以及compileSdkVersion为28以上。（否则点击Migrate to Androidx会出现如下错误）*

![](https://img-blog.csdnimg.cn/2019071109224159.png)


然后手动检查项目的并修改，先 build会报很多的错，慢慢改。
### *没有或不能适配AndroidX的插件包，建议能删掉的就删掉*