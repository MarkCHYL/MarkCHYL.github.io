---
layout: post
title: MAC使用APKTool反编译apk修改版本号后重新打包
toc: true
reward: true
date: 2021-04-14 16:49:22
tags: [工具搭建,反编译]
categories: [Android]
---
现在业务上出现客户要求如若生产环境出现紧急事件，需要版本回退的情况下，我这边的代码又没做版本分之处理，那么只能更改之前的版本号来实现。
[不敢夺他人之功，原文在此处](https://blog.csdn.net/weixin_40998254/article/details/110474885)

[我之前的文章：android反编译apktool---dex2jar---jdgui](https://markchyl.cn/2020/12/14/android%E5%8F%8D%E7%BC%96%E8%AF%91apktool-dex2jar-jdgui/)

### 一、反编译apk并修改版本号
在apk所在目录控制台输入下面指令，即可将文件名为source的apk反编译到outDir目录
* 开始反编译apk
  
  ```apktool d -o outDir source.apk```
  或者
  ```apktool d source.apk -o outDir```

### 二、修改版本号
打开输入目录outDir找到apktool.yml文件，编辑修改versionCode

### 三、重新打包
通过以下命令就可以将目录outDir中的文件重新打包为no_sign_result.apk
* ```apktool b -o no_sign_result.apk outDir```

或者

* ```apktool b outDir -o  no_sign_result.apk ```
  
### 四、重新签名
使用如下命令进行签名
```jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore demostore.jks -signedjar result-signed.apk no_sign_result.apk yourkey```
注：
* demostore.jks为签名文件
* no_sign_result.apk为要签名的源文件
* result-signed.apk为签名后的目标文件
* yourkey为签名的key

