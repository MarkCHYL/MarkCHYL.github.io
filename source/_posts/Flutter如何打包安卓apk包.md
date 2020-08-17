---
layout: post
title: Flutter如何打包安卓apk包
toc: true
reward: true
date: 2019-11-14 09:05:43
tags: [Flutter]
---

作为安卓原生开发多年的我，也是第一次接触这种方式配置打包信息。原谅我的无知。
### 一、在项目根目录下新建 key.properties 文件
配置签名的基本信息
<!--more-->
```
storePassword=chenyunlin
keyPassword=chenyunlin
keyAlias=Mark
storeFile=/Users/mark/Desktop/Document/keyStore/MarkKey.jks
```
### 二、配置到app目录下的 build.gradle 文件
在安卓目录下添加如下配置：导入配置 key.properties 的路径
```
def keystorePropertiesFile = rootProject.file("key.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
android{
    ······
}
```
配置签名的信息：
```
android {
    ····
      signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
        debug {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }
    ····
}
```

这样下来安卓的配置信息配置完成。
下面修改buildType是：
```
 buildTypes {
        release {
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig signingConfigs.release
        }
        debug {
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig signingConfigs.debug
        }
    }
```
### 三、切换到Terminal中
输入 `Flutter build apk`,请静待程序运行完成。
