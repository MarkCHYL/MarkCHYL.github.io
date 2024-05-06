---
layout: post
title: 在Ubuntu上安装多个版本的Java JDK
toc: true
reward: true
date: 2023-09-08 11:28:36
tags: [环境配置]
categories: [ubuntu]
---
在Ubuntu上安装多个版本的Java JDK（Java Development Kit）是可行的，并且可以使用`update-alternatives`来管理不同版本之间的切换。以下是一个示例，演示如何在Ubuntu上安装和管理多个Java JDK版本。

1. **检查系统上已安装的Java版本：**

   在终端中运行以下命令，以查看系统上已安装的Java版本：

   ```bash
   update-java-alternatives -l
   ```
<!-- more -->
   这将列出已安装的Java版本。

2. **安装新的Java JDK版本：**

   可以使用`apt`或手动下载并安装Java JDK。以下是使用`apt`来安装OpenJDK 8和OpenJDK 11的示例：

   ```bash
   # 安装OpenJDK 8
   sudo apt update
   sudo apt install openjdk-8-jdk

   # 安装OpenJDK 11
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```

   您可以根据需要安装其他版本的Java JDK。

3. **使用`update-alternatives`选择默认版本：**

   使用`update-alternatives`命令选择默认的Java版本。例如，选择OpenJDK 11作为默认版本：

   ```bash
   sudo update-alternatives --config java
   ```

   这将列出系统上已安装的Java版本，并要求您选择默认版本。按照提示进行选择。

4. **验证默认版本：**

   使用以下命令验证默认的Java版本：

   ```bash
   java -version
   ```

   这将显示当前默认的Java版本。

5. **切换Java版本：**

   如果需要切换到其他已安装的Java版本，可以随时使用`update-alternatives`命令进行更改。例如，要切换回OpenJDK 8：

   ```bash
   sudo update-alternatives --config java
   ```

   然后选择OpenJDK 8。

通过这些步骤，您可以在Ubuntu上安装和管理多个Java JDK版本，并随时切换到需要的版本。