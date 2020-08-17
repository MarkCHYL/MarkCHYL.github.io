---
layout: post
title: Dart的基础知识学习
date: 2018-12-17 11:13:20
toc: true
reward: true
tags: [Flutter]
---
### 简介：
        Google和其他地方的开发人员使用Dart为iOS，Android和网络创建高质量，关键任务的应用程序。Dart具有针对客户端开发的功能，非常适合移动和Web应用程序。
        Dart对于许多现有的开发人员来说都很熟悉，这要归功于它不出异的面向对象和语法。如果您已经了解C ++，C＃或Java，那么只需几天就可以使用Dart。
        Dart编译为ARM和x86代码，因此Dart移动应用程序可以在iOS，Android及更高版本上本机运行。对于Web应用程序，Dart会转换为JavaScript。
        Dart提供优化的提前编译，以在移动设备和Web上实现可预测的高性能和快速启动。

* * *
 <!--more-->
### 工具：
![https://note.youdao.com/yws/api/personal/file/WEB2e99b7c66fa25ec939b46b986618bc0e?method=download&shareKey=6e3cd2073bc556b731b1d3a68a1c6acf](https://note.youdao.com/yws/api/personal/file/WEB2e99b7c66fa25ec939b46b986618bc0e?method=download&shareKey=6e3cd2073bc556b731b1d3a68a1c6acf)
##### 无需安装就可以验证编译代码运行。 
DartPad是一种很好的，无需下载的方法来学习Dart语法和试验Dart语言功能。它支持Dart的核心库，但dart：io等VM库除外。[链接](https://dartpad.dartlang.org/)
IDE和编辑器
这些常用的IDE存在Dart插件。
![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=1105042996,1878674339&fm=58&bpow=496&bpoh=405)
 Android Studio
 ![3aaf76a25ee53fd2b4c50db1cc857576.svg+xml](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=330314814,3181569317&fm=58&bpow=310&bpoh=270)
  IntelliJ IDEA （和其他JetBrains IDE）
  ![](http://cms-bucket.nosdn.127.net/catchpic/9/9a/9aa087361923a8896e8c0f5f6dd22b05.jpg?imageView&thumbnail=550x0)
   Visual Studio代码
* * *
### HelloWorld：
```
void main() {
  print('Hello, World!');
}
```
### 默认值
        未初始化的变量默认值是 null。即使变量是数字 类型默认值也是 null，因为在 Dart 中一切都是对象，数字类型 也不例外



