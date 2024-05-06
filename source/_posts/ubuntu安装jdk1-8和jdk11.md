---
layout: post
title: ubuntu安装jdk1.8和jdk11
toc: true
reward: true
date: 2023-06-29 10:17:59
tags: [环境配置]
categories: [Linux]
---

如果您需要在Ubuntu虚拟机上同时安装JDK 1.8和JDK 11，您可以按照以下步骤进行操作：

1. 安装JDK 1.8：
   打开终端，运行以下命令安装OpenJDK 8：

   ```
   sudo apt update
   sudo apt install openjdk-8-jdk
   ```

   安装完成后，可以使用`java -version`命令验证安装的JDK版本。
<!-- more -->
2. 安装JDK 11：
   打开终端，运行以下命令安装OpenJDK 11：

   ```
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```

   安装完成后，可以使用`java -version`命令验证安装的JDK版本。

3. 配置默认JDK版本：
   如果您需要将JDK 1.8或JDK 11设置为默认的Java版本，可以使用`update-alternatives`命令进行配置。运行以下命令选择默认的Java版本：

   ```
   sudo update-alternatives --config java
   ```

   系统将显示可用的Java版本列表，并要求您选择默认版本。根据提示输入相应的数字，然后按Enter键进行选择。

现在，您的Ubuntu虚拟机已经同时安装了JDK 1.8和JDK 11，并且您可以根据需要选择默认的Java版本。