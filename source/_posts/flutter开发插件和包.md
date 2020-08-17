---
layout: post
title: flutter开发插件和包
date: 2019-07-02 09:43:22
toc: true
reward: true
tags: [Flutter]
---

## cmd 命令创建开发插件包
 `flutter create --org com.mark --template markplugin`
  使用该--org选项以反向域名表示法指定您的组织。此值用于生成的Android和iOS代码中的各种包和包标识符。
  其中 com.mark 就是我的组织名，markplugin 就是我的插件项目名字
  生成如下：
<!--more-->
!["插件包生成内容"](https://img-blog.csdnimg.cn/20190702095035265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01hcmtfQ0hZTA==,size_16,color_FFFFFF,t_70  )

***默认情况下，插件项目使用Objective-C for iOS代码和Java for Android代码。如果您更喜欢Swift或Kotlin，您可以使用-i和/或使用Android语言指定iOS语言-a.***

例如：
` flutter create --org com.mark --template=plugin -i swift -a kotlin markkotlinpugin`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190702101606454.png)

## 如何通过命令创建Flutter包
要创建Dart包，请使用以下--template=package标志flutter create：
 `flutter create --org com.mark --template=package hello`

这将在hello/文件夹中创建一个包项目，其中包含以下专门内容：

`
lib/hello.dart：包的Dart代码。

test/hello_test.dart：该单元测试包装。`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190702102101747.png)

## 配置好pubspec.paml上的信息,添加开源协议，编写使用说明
```
name: flutter_tools
description: A new Flutter Tools package. it works on ios and android
version: 0.0.1
author: MarkCHYL <2285581945@qq.com>
homepage: https://github.com/MarkCHYL/flutter_tools
```
## 发布包
[参考地址](https://flutter.dev/docs/development/packages-and-plugins/developing-packages)
首先我们执行：
`flutter pub pub publish --dry-run`
会检查发布包时是否准备好，是否有错误。
`flutter packages pub publish`发布包，
但是我因网络原因，没上传成功过。首次大包上传需要登陆谷歌的网站，谷歌账号登陆便好。