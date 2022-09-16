---
layout: post
title: Mac中终端关机命令
toc: true
reward: false
date: 2021-11-02 10:10:25
tags: [运维]
categories: [Mac]
---

[原文来自](https://blog.51cto.com/59465168/1837538)
1.立即关机命令 
        sudo halt 
    或者 
        sudo shutdown -h now 
 
2. 10分钟后关机 
   sudo shutdown -h +10 
 
3. 晚上8点关机 
sudo shutdown -h 20:00 
 
4. 立即重启 
    sudo reboot  
    或者 
    sudo shutdown -r now

5.设定时间为2012年7月12日15：00分关机,命令为：
    sudo shutdown -h 1207121500 
同理： 
    2014年7月11日15：00分重启,命令：
    sudo shutdown -r 1407111500 
 命令的主体位：shutdown（关闭） 
     h/r/s -->分别代表：关机/重启/睡眠。 
 最后加上时间就可行了。