---
layout: post
title: ubuntu20.04虚拟机安装redis-3.2.8
toc: true
reward: true
date: 2023-08-11 10:30:33
tags: [Redis,Linux]
categories: [中间件]
---
安装 Redis-3.2.8 在 Ubuntu 20.04 虚拟机上可以按照以下步骤进行：

## 安装

1. 在终端中安装 `build-essential` 和 `tcl`：

   ```
   sqlCopy code
   sudo apt-get update
   sudo apt-get install build-essential tcl
   ```
<!-- more -->
2. 下载 Redis-3.2.8 并解压缩：

   ```
   bashCopy code
   wget http://download.redis.io/releases/redis-3.2.8.tar.gz
   tar xzf redis-3.2.8.tar.gz
   cd redis-3.2.8
   ```

3. 编译 Redis：

   ```
   goCopy code
   make
   ```

4. 运行 Redis 测试：

   ```
   bashCopy code
   make test
   ```

5. 安装 Redis：

   ```
   goCopy code
   sudo make install
   ```

6. 创建 Redis 配置文件目录和配置文件：

   ```
   bashCopy code
   sudo mkdir /etc/redis
   sudo cp redis.conf /etc/redis
   ```

7. 修改 Redis 配置文件：

   ```
   bashCopy code
   sudo nano /etc/redis/redis.conf
   ```

   找到 `bind` 行并将其注释掉，以允许远程访问 Redis：

   ```
   shellCopy code
   # bind 127.0.0.1
   ```

   找到 `protected-mode` 行并将其改为 `no`，以允许远程访问 Redis：

   ```
   arduinoCopy code
   protected-mode no
   ```

8. 启动 Redis 服务器：

   ```
   bashCopy code
   redis-server /etc/redis/redis.conf
   ```

   如果出现错误，请查看 Redis 日志文件 `/var/log/redis.log`。

现在你应该已经成功安装和启动了 Redis-3.2.8 服务器。你可以通过运行 `redis-cli` 命令来测试 Redis 是否可以正常工作。

## 补充配置

- 绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip

  > bind 127.0.0.1

- 端⼝，默认为6379

  > port 6379

- 是否以守护进程运⾏

  - 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
  - 如果以⾮守护进程运⾏，则当前终端被阻塞
  - 设置为yes表示守护进程，设置为no表示⾮守护进程
  - 推荐设置为yes

  > daemonize yes

- 数据⽂件

  > dbfilename dump.rdb

- 数据⽂件存储路径

  > dir /var/lib/redis

- ⽇志⽂件

  > logfile /var/log/redis/redis-server.log

- 数据库，默认有16个

  > database 16

- 主从复制，类似于双机备份。

  > slaveof

## 加载配置启动时报错

> 'logfile /var/log/redis/redis-server.log' Can't open the log file: Permission denied

这个错误提示表明 Redis 没有权限在 `/var/log/redis/` 目录中创建日志文件。

你可以按照以下步骤来解决这个问题：

1. 确认 `/var/log/redis/` 目录是否存在，如果不存在，你可以通过以下命令创建它：

   ```
   bashCopy code
   sudo mkdir /var/log/redis/
   ```

2. 确认 Redis 进程的运行用户是否有权限在该目录中创建文件。默认情况下，Redis 进程会使用 `redis` 用户来运行，而该用户通常没有权限在 `/var/log/redis/` 目录中创建文件。

   如果你想让 `redis` 用户有权限在该目录中创建文件，可以通过以下命令更改该目录的所有者和组：

   ```
   bashCopy code
   sudo chown redis:redis /var/log/redis/
   ```

   这将把 `/var/log/redis/` 目录的所有者和组都更改为 `redis`。

   如果你不希望更改该目录的所有者和组，你可以考虑在 Redis 配置文件中指定其他的日志文件路径，例如 `/tmp/redis-server.log`：

   ```
   bashCopy code
   logfile /tmp/redis-server.log
   ```

3. 如果你更改了 Redis 配置文件，记得重新启动 Redis 服务，以使更改生效：

   ```
   Copy code
   sudo systemctl restart redis-server
   ```

如果你遇到了其他问题或错误，请查看 Redis 的日志文件和系统日志文件以获取更多信息。