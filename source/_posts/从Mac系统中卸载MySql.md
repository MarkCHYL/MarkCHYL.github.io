---
layout: post
title: 从Mac系统中卸载MySql
toc: true
reward: false
date: 2021-11-03 10:26:24
tags: [mysql]
categories: [数据库]
---
[原文](https://blog.csdn.net/lancegentry/article/details/79144131)

```
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/MySQL*
sudo rm -rf ~/Library/PreferencePanes/MySQL*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
```
最后删除/etc/hostconfig文件中的  MYSQLCOM=-YES-

这个选项有些用户有，有些没有

最后如果系统偏好设置中没有了MySql证明卸载成功