---
layout: post
title: 'LinuxDeploy下ping命令无法运行,提示socket:权限不足'
toc: true
reward: true
date: 2022-10-30 23:44:12
tags: [命令,Linux]
categories: [网络]
---
```vim
ping命令需要SUID权限的
检查以下它的权限位是否是-rwsr-xr-x，并且owner是root
如果没有那个s，
# chmod u+s /bin/ping
如果owner不是root
# chown root /bin/ping
```