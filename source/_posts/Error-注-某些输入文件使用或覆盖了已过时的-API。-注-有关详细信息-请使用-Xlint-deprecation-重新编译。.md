---
layout: post
title: 'Error:注: 某些输入文件使用或覆盖了已过时的 API。 注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。'
toc: true
reward: true
date: 2019-12-12 16:33:40
tags: [Error]
categories: [Android]
---
[原文](https://blog.csdn.net/dong19900415/article/details/52882529?utm_source=blogxgwz3)

Error:注: 某些输入文件使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
注: 某些输入文件使用了未经检查或不安全的操作。
注: 有关详细信息, 请使用 -Xlint:unchecked 重新编译。
FAILURE: Build failed with an exception.
最近项目出现的一些问题可以在build中加入

```
allprojects {
    gradle.projectsEvaluated {
        tasks.withType(JavaCompile) {
            options.compilerArgs << "-Xlint:unchecked" << "-Xlint:deprecation"
        }
    }
}
```