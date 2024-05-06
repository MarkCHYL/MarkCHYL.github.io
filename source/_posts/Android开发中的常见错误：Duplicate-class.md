---
layout: post
title: Android开发中的常见错误：Duplicate class
toc: true
reward: true
date: 2023-02-21 09:17:32
tags: [bug]
categories: [Android]
---

## android开发Duplicate class编译出现类重复问题的定位以及解决方法

>Task :app:checkReleaseDuplicateClasses FAILED

>Duplicate class com.google.common.util.concurrent.ListenableFuture found in modules jetified-guava-20.0 (com.google.guava:guava:20.0) and jetified-listenablefuture-1.0 (com.google.guava:listenablefuture:1.0)
```

### 解决方法：

    1. Navigation -> Search Everywhere，勾选Include non-project items进行全局搜索
    2. 比如搜索com.google.guava就可以搜索出所有的有关的依赖库
    3. ./gradlew app:dependencies查看找出依赖库之间的关系(在gradle中执行)
    4. 找到有关的依赖库xxx:xx:x
    ```
        implementation("xxx:xx:x") {
            exclude group: 'com.google.guava'
        }
        或者android配置区域配置全部库排查如下：
        configurations.all {
            exclude group: 'com.google.guava', module:'listenablefuture'
        }
    ```

### zsh: permission denied: ./gradlew
```
运行: chmod +x gradlew
```