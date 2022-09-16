---
layout: post
title: Centos7 配置ssh免密登陆以及遇到的问题
toc: true
reward: true
date: 2021-09-17 16:28:39
tags: [网络配置]
categories: [Linux]
---
假设用户名为**u**：
* 1.确认已经连接上互联网，然后输入命令：
  `sudo apt-get install ssh`
* 2.配置为可以免密码登录本机。首先查看在u用户下是否存在.ssh文件夹（注意ssh前面有“.”，这是一个隐藏文件夹），输入命令：
    `ls –a /home/u`

    >一般来说，安装SSH时会自动在当前用户下创建这个隐藏文件夹，如果没有，可以手动创建一个。

* 3.接下来，输入命令（注意下面命令中不是双引号，是两个单引号）：
`ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa`

>解释一下，ssh-keygen代表生成密钥；-t（注意区分大小写）表示指定生成的密钥类型；dsa是dsa密钥认证的意思，即密钥类型；-P用于提供密语；-f指定生成的密钥文件。

>在Linux系统中，~代表当前用户文件夹，此处即/home/u。

>这个命令会在.ssh文件夹下创建id_dsa及id_dsa.pub两个文件，这是SSH的一对私钥和公钥，类似于钥匙和锁，把id_dsa.pub（公钥）追加到授权的key中去。
输入命令:

`cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys`

这条命令的功能是把公钥加到用于认证的公钥文件中，这里的authorized_keys是用于认证的公钥文件。

至此免密码登录本机已配置完毕。

* 4.验证SSH是否已安装成功，以及是否可以免密码登录本机。
输入命令：

ssh –version

显示结果：

OpenSSH_5.8p1 Debian-7ubuntu1, OpenSSL 1.0.0e 6 Sep 2011

Bad escape character 'rsion'.

显示SSH已经安装成功了。

输入命令：
`ssh localhost`
会有如下显示：
```
The authenticity of host 'localhost (::1)' can't be established.
RSA key fingerprint is 8b:c3:51:a5:2a:31:b7:74:06:9d:62:04:4f:84:f8:77.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (RSA) to the list of known hosts.
Linux master 2.6.31-14-generic #48-Ubuntu SMP Fri Oct 16 14:04:26 UTC 2011 i686
To access official Ubuntu documentation, please visit:
http://help.ubuntu.com/
Last login: Sat Feb 18 17:12:40 2012 from master
```
第一次登录时会询问是否继续链接，输入yes即可进入。