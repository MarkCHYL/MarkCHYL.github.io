---
layout: post
title: Mac 每次都要执行source ~/.bash_profile 配置的环境变量才生效
toc: true
reward: true
date: 2020-07-28 09:02:31
tags: [系统]
categories: [工具使用]
---
### 问题
遇到一个问题，前段时间由于系统升级之后，做了一下用户群组的改动，在开发的时候发现之前在
`.bash_profile`中设置的环境变量都不见了，还只能在终端中执行一次`source .bash_profile`,环境变量才能生效。
### 解决办法
在系统根目录下，在~/.zshrc文件最后，增加一行：
source ~/.bash_profile