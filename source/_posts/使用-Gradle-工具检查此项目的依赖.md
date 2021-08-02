---
layout: post
title: 使用 Gradle 工具检查此项目的依赖
toc: true
reward: true
date: 2020-12-18 15:48:51
tags: [Android,Gradle]
categories: [Android]
---
使用 Gradle 工具检查此项目的依赖，进入项目目录，执行如下指令进行依赖检查：
```
cd app
gradle dependencies

```
打印出如下图所示的依赖树，依赖树显示了你 build 脚本声明的顶级依赖和它们的传递依赖：
```
+--- androidx.test.ext:junit:1.1.1
|    +--- junit:junit:4.12
|    |    \--- org.hamcrest:hamcrest-core:1.3
|    +--- androidx.test:core:1.2.0
|    |    +--- androidx.annotation:annotation:1.0.0 -> 1.1.0
|    |    +--- androidx.test:monitor:1.2.0
|    |    |    \--- androidx.annotation:annotation:1.0.0 -> 1.1.0
|    |    \--- androidx.lifecycle:lifecycle-common:2.0.0 -> 2.1.0
|    |         \--- androidx.annotation:annotation:1.1.0
|    +--- androidx.test:monitor:1.2.0 (*)

```