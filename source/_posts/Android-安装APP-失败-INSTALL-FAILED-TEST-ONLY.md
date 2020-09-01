---
layout: post
title: Android 安装APP 失败 INSTALL_FAILED_TEST_ONLY
toc: true
reward: true
date: 2020-08-07 09:40:00
tags: [Android,Error]
categories: [Android]
---
### android studio 进行真机安装时失败，报错：
> Installation did not succeed.
The application could not be installed: INSTALL_FAILED_TEST_ONLY
Installation failed due to: 'null'

### 解决办法
在`gradle.properties 文件中`添加

>android.injected.testOnly=false