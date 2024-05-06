---
layout: post
title: 'shell脚本提示 ll:command not found'
toc: true
reward: true
date: 2023-03-06 10:26:25
tags: [Error]
categories: [Linux]
---
shell脚本test.sh代码：

```shell
#!/bin/sh

# 查看test目录下的文件列表
ll /test
```
执行报错：
```shell
test.sh: line 3: ll: command not found
```
解决方法：
```shell
#!/bin/sh

# 查看test目录下的文件列表
ls -l /test
```