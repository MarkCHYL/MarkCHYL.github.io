---
layout: post
title: Charles抓包工具在mac上如何配置
toc: false
reward: false
date: 2019-12-18 13:25:37
tags: [工具使用]
categories: [工具使用]
---
#### 配置：
* 电脑：MacBook Pro (13-inch, 2017, Two Thunderbolt 3 ports)
* 手机：安卓手机4.4版本（是定制的）
* Charles版本：v4.5.5
* 手机与macbook需要连接同一网段的网络，macbook可以是有线，手机连wifi，也可以两者连接同一个wifi。

***
### 第一、电脑上安装Charles
官网下载安装：[官网地址](https://www.charlesproxy.com/download/)
下载后安装
### 第二、设置charles代理
打开charles/proxy/proxy-settings，设置一个端口号，默认的8888就可以。
![网图盗用下](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/accxv-s218u.jpg?raw=true)

### 第三、手机安装charles证书
需要安装charles的证书。点击help/SSL proxying

![](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/aa1g5-ewu90.jpg?raw=true)
查询macbook的ip地址，并在手机连接的wifi上手动设置代理，代理主机名为ip地址，代理端口号为8888，会弹出一个框，显示的意思是手机上的wify需要设置代理。

### 第三、手机设置代理

查询macbook的ip地址，并在手机连接的wifi上手动设置代理，代理主机名为ip地址，代理端口号为8888，这时候用手机访问网页，charles会弹出下列框，说明charles已经开始对手机抓包了，点击允许。

![](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/a1evd-z9c3a.jpg?raw=true)

然后在手机浏览器中访问手机http://charlesproxy.com/getssl，安装即可，
![](https://github.com/MarkCHYL/BLOG/blob/master/marksource/images/a921v-hyhye.jpg?raw=true)


好了，现在就可以流畅的抓取手机上的各种http/https请求了，想要学习更多charles工具方法.


本人不做推广，只是供自己即好友参考做的笔记
感谢开发员：[小小的开发人员](https://www.jianshu.com/p/50f844c9beaf)