---
layout: post
title: Mac安装mysql-8.0.27
toc: true
reward: false
date: 2021-11-03 10:33:36
tags: [mysql]
categories: [数据库]
---
### 我的环境：
*  MacOs Big Sur 版本11.6
*  mysql-8.0.27-macos11-x86_64.dmg

### 下载安装

版本一定要下对，我踩过坑，

![](https://markchyl.cn/img/20180123204406478.png)

如图所示来查找对应系统安装版本，
[官网下载地址是](https://dev.mysql.com/downloads/mysql/)

### 配置环境变量
打开终端
输入：cd /usr/local/mysql，回车执行
然后输入：sudo vim .bash_profile，回车执行
```
<!-- more -->
#mysql环境变量
export MYSQL_HOME=/usr/local/mysql
export PATH=${MYSQL_HOME}/bin:$PATH
```
记得`source ~/.bash_profile`让环境变量生效。

最后使用输入mysql命令`mysql -u root -p`，即可使用。