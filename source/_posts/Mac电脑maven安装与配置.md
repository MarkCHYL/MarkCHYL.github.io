---
layout: post
title: Mac电脑maven安装与配置
toc: true
reward: true
date: 2021-05-13 10:39:04
tags: [Maven]
categories: [环境配置]
---
[原文地址：大自然的流风](https://www.cnblogs.com/zdz8207/p/mac-java-maven.html)
***
### Mac电脑maven安装与配置
1. 下载：http://maven.apache.org/download.cgi
2. 安装：解压下载好的maven的文件，解压到你想要的文件夹下。
3. 配置：打开终端输入命令 sudo vim ~/.bash_profile （编辑环境变量配置文件）
   ```
   export MAVEN_HOME=maven文件夹路径
   export PATH=$PATH:$MAVEN_HOME/bin
   ```
   **小技巧**
   >复制文件夹路径方法：点击文件夹，然后使用组合快捷键：command + option + c 就会把路径复制到粘贴板了。
或者把文件夹拖放到控制台也可以显示出来
让mac文件夹显示文件夹和文件路径，执行命令：defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES

4. `:wq`退出并保存当前文件,`source .bash_profile`，按下Enter键使bash_profile生效。
   `mvn -v`查看是否成功
```
@Markxiansheng ~ % mvn -v
Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)
Maven home: /Users/mark/Library/maven/apache-maven-3.8.1
Java version: 11.0.11, vendor: Amazon.com Inc., runtime: /Users/mark/Library/Java/JavaVirtualMachines/corretto-11.0.11/Contents/Home
Default locale: zh_CN_#Hans, platform encoding: UTF-8
OS name: "mac os x", version: "10.15.7", arch: "x86_64", family: "mac"
```