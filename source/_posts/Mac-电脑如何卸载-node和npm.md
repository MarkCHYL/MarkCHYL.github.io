---
layout: post
title: Mac 电脑如何卸载 node和npm
toc: true
reward: false
date: 2020-08-25 10:37:35
tags: [系统]
---
记录下我自己重新安装npm，因为npm是和node.js安装一起的，也就是说是重新弄*node*
<!-- more -->
### 一、在终端依次输入以下命令
```
 sudo npm uninstall npm -g
 sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
 sudo rm -rf /usr/local/include/node /Users/$USER/.npm
 sudo rm /usr/local/bin/node
 sudo rm /usr/local/share/man/man1/node.1
```
### 二
输入 npm -v 和node -v 验证是否卸载成功。