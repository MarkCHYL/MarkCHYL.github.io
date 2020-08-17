---
layout: post
title: Linux命令学习
toc: true
reward: true
date: 2020-04-09 00:28:01
tags: [Linux]
---

## 远程管理命令
> ### **关机/重启**

| 序号 | 命令 | 对应英文 | 作用 |
|--|--|--|--|
| 01 | shutdown 选项 时间 | shutdown | 关机/重新启动 |
* shutdown 命令可以安全 **关闭** 或 **重启系统**

|选项| 含义|
|--|--|
|-r|重新启动|

> 提示：
> * 不指定选项和参数，默认表示 1 分钟后关闭电脑
> * 远程维护服务器时，最好不要关闭系统，而是应该重启系统

```

```

### ssh客户端的简单实用
> ssh [-p port] user@remote
如果服务器端没有设置账号密码，或者是无密码登录，直接写`ssh -p 22 192.168.177.156`或`ssh -p 228 mark@192.168.177.156`

关于ssh配置方法：
ubuntu下默认是不允许root通过密码的方式通过ssh远程登录服务器的，可以通过在
```sudo vi /etc/ssh/sshd_config
#增加以下配置允许通过ssh登录

#PermitRootLogin prohibit-password
PermitRootLogin yes

#修改完成后需要重启ssh服务命令如下
sudo service ssh restart
```
即可通过ssh的root用户登录服务器了。
下面说下如何修改root密码

```sudo passwd root
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
su root #即生效
```
此时若想验证看root密码是否更改成功，可以通过如下命令
```
su - root
#在下方输入修改后的密码，输入后回车
Password:
```
