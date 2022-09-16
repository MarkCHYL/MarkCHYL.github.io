---
layout: post
title: mac 安装homebrew
toc: true
reward: true
date: 2021-10-09 09:57:26
tags: [工具使用]
categories: [工具使用]
---
问题：macOS安装Homebrew时总是报错（Failed to connect to raw.githubusercontent.com port 443: Connection refused）

原因：由于某些你懂的因素，导致GitHub的raw.githubusercontent.com域名解析被污染了。

解决办法：通过修改hosts解决此问题。
查询真实IP

在https://www.ipaddress.com/查询raw.githubusercontent.com的真实IP。
![](https://imgconvert.csdnimg.cn/aHR0cDovL29zY2ltZy5vc2NoaW5hLm5ldC9vc2NuZXQvdXAtNzMwZGI4MDg5OGIzYmI5NzU2NDdlOTk1NzViZTEyZWY4N2EucG5n?x-oss-process=image/format,png)
修改hosts
```linux
sudo vim /etc/hosts
```
添加如下内容：
```
199.232.28.133 raw.githubusercontent.com
```
可以使用国内源啦
```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```