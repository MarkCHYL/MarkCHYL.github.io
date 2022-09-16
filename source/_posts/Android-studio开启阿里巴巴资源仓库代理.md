---
layout: post
title: Android studio开启阿里巴巴资源仓库代理
toc: true
reward: true
date: 2022-09-16 22:49:30
tags: [Android]
categories: [Android]
---
```java
repositories {
    maven {
        //解决 gradle.build 仓库更换为阿里云仓库后报错
        allowInsecureProtocol = true
        //阿里云仓库
        url 'http://maven.aliyun.com/nexus/content/groups/public/'
    }
   。。。。。。
}
```