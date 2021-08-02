---
layout: post
title: Mac用Docker安装Oracle11g并连接Navicat
toc: true
reward: true
date: 2020-12-01 09:04:34
tags: [环境搭建,Oracle]
categories: [数据库]
---
[原文转载自大佬笑等茶凉](https://www.cnblogs.com/lihanqing/p/12329480.html)
### 1.下载并安装Docker

官方下载地址：https://download.docker.com/mac/stable/Docker.dmg

### 2.用docker下载镜像，在终端输入：

>docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

### 3.启动oracle镜像作为容器：

>docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

### 4.进入镜像配置

>docker exec -it oracle11g bash
<!-- more -->

### 5.配置环境变量

```
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH
```
### 6.修改密码
>sqlplus /nolog

>SQL> conn /as sysdba;
SQL> alter user system identified by oracle;
SQL> conn system/oracle;
那样便修改了账号为system密码为oracle的账号.

**系统权限管理 :**
  
  * 系统权限分类：
  DBA: 拥有全部特权，是系统最高权限，只有DBA才可以创建数据库结构。
RESOURCE:拥有Resource权限的用户只可以创建实体，不可以创建数据库结构。
CONNECT:拥有Connect权限的用户只可以登录Oracle，不可以创建实体，不可以创建数据库结构。
对于普通用户：授予connect, resource权限。
对于DBA管理用户：授予connect，resource, dba权限。
****
  * 系统权限授权命令：
  系统权限只能由DBA用户授出：sys, system(最开始只能是这两个用户)
授权命令：SQL> grant connect, resource, dba to 用户名1 [,用户名2]…;
注:普通用户通过授权可以具有与system相同的用户权限，但永远不能达到与sys用户相同的权限，system用户的权限也可以被回收。

### 7.创建用户
>SQL> create user mark identified by chenyunlin;

用sysdba赋予该用户所有权限：
>SQL> grant all privileges to mark;

连接新创建的用户：
>SQL> conn mark/chenyunlin;

创建表：
>SQL> create table test2(name varchar2(20), city varchar2(20));

比如在此你拥有自动化创建数据表的脚本的话
`## 执行sql脚本文件
@/Users/mark/Desktop/iotek_oracle.sql;`

### 8.使用Navicat连接oracle

图片借用哈
![](https://img2018.cnblogs.com/blog/1384393/202002/1384393-20200219000941495-1048166820.png )

注意：服务名helowin是镜像地址中的

**常用的一些命令：**

`docker ps`是查看当前运行的容器

`docker ps -a `是查看所有容器（包括停止的）

`docker images`查看所有镜像

`docker run -h "oracle" --name "oracle" -d -p 49160:22 -p 49161:1521 -p 49162:8080 alexeiled/docker-oracle-xe-11g`
​ -h "oracle"：指定容器的hostname为oracle

　　--name "oracle"：将容器命名为oracle

　　-d：在后台运行

　　-p: 端口映射，格式为：主机(宿主)端口:容器端口

**启动或停止oracle服务：**

`docker start oracle11g`

`docker stop oracle11g`

### 9.后续操作命令
删除容器：
>docker rm [containerId]
删除镜像：
>docker rmi [imageId]

### 10.使用终端打开sqlplus 
由于本人电脑的配置原因，
*  docker exec -it oracle11g bash
```
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH
sqlplus /nolog

SQL*Plus: Release 11.2.0.1.0 Production on Fri Dec 4 15:03:36 2020

Copyright (c) 1982, 2009, Oracle.  All rights reserved.

SQL> 
```
