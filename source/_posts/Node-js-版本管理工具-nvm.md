---
layout: post
title: 'Node.js 版本管理工具:nvm'
toc: true
reward: true
date: 2024-06-13 11:17:19
tags: [环境]
categories: [开发工具]
---
[参考原文](https://blog.csdn.net/qq_41581588/article/details/139227504)

nvm 是一款 Node.js 版本管理工具，允许用户通过命令行快速安装、切换和管理不同的 Node.js 版本。

图片![](https://img-blog.csdnimg.cn/img_convert/49f67ddef04c0f95bf84de1a72a19853.png)
（图片来自：github）

>nvm 只适用于 macOS 和 Linux 用户的项目，如果是 Windows 用户，可以使用 nvm-windows 、nodist 或 nvs 替换。

<!-- more -->

### 安装方式
macOS 下载方式：

```shell
# 方式1 浏览器打开下面链接下载
https://github.com/nvm-sh/nvm/blob/v0.39.7/install.sh
# 下载完成后，通过命令安装
sh install.sh
 
# 方式2 推荐
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
 
# 方式3
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
安装过程中如果遇到一些奇怪的问题，可以查看下 nvm 补充说明

### 常用命令
```shell
nvm ls                # 查看版本安装所有版本
nvm ls-remote         # 查看远程所有的 Node.js 版本
nvm install 17.0.0    # 安装指定的 Node.js 版本
nvm use 17.0.0        # 使用指定的 Node.js 版本
nvm alias default 17.0.0  # 设置默认 Node.js 版本
nvm alias dev 17.0.0  # 设置指定版本的别名，如将 17.0.0 版本别名设置为 dev
```
