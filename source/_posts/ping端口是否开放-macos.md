---
layout: post
title: 'ping端口是否开放(macos)'
toc: true
reward: true
date: 2019-10-30 17:01:33
tags: [网络编程]
---
*简单的实用积累*

[原文转自](https://blog.csdn.net/woodwindforward/article/details/89641636)
### mac中：
打开终端：

* 输入 ping 域名：ping www.baidu.com
  
  ![](https://img2018.cnblogs.com/blog/1368523/201904/1368523-20190425193812807-428916356.png)

* ping端口：nc -vz -w 2 www.baidu.com 8080
  
  ![](https://img2018.cnblogs.com/blog/1368523/201904/1368523-20190425193241938-258296742.png)
  