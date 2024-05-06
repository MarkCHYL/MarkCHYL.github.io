---
layout: post
title: Mac安装Redis
toc: true
reward: true
date: 2023-08-11 10:29:03
tags: [Redis,Mac]
categories: [中间件]
---
要在Mac上安装`Redis`，请按照以下步骤进行操作：

1. 打开终端应用程序

2. 安装`Homebrew，Homebrew`是一个Mac上的包管理器。在终端中运行以下命令：

   `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
<!-- more -->

3. 安装`Redis`，运行以下命令：

   `brew install redis`

4. 启动`Redis`，运行以下命令：

   `redis-server`

5. （可选）如果您想在后台运行`Redis`，请使用以下命令：

   `redis-server --daemonize yes`

6. （可选）如果您想在每次启动Mac时自动启动`Redis`，请使用以下命令：

   `brew services start redis`

   `brew services stop redis `

现在您已经成功地安装和启动了`Redis`。您可以在终端中使用`redis-cli`命令来连接到`Redis`服务器并进行交互式操作。