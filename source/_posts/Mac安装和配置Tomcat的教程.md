---
layout: post
title: Mac安装和配置Tomcat的教程
toc: true
reward: false
date: 2021-11-02 13:11:06
tags: [Tomcat]
categories: [JavaWeb]
---
[原文](https://blog.csdn.net/dongzhensong/article/details/87807378)
### 我的配置
* MacBook Pro
* jdk1.8
### 1.下载
前往[ApacheTomcat](https://tomcat.apache.org/download-80.cgi)官网下载Tomcat：
![](https://img-blog.csdnimg.cn/20190220165426253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rvbmd6aGVuc29uZw==,size_16,color_FFFFFF,t_70)
首先选择相应的版本（以Tomcat 8为例）：
下载右边Core下的第一个资源zip。
下载后解压下来重名名为ApacheTomcat，并放到磁盘的/usr/local下
![](https://img-blog.csdnimg.cn/20190220165752464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rvbmd6aGVuc29uZw==,size_16,color_FFFFFF,t_70)
<!-- more -->
### 2.启动服务
打开终端.app，切换路径到ApacheTomcat的bin目录下并执行启动文件：
```
mark@localhost bin % cd /usr/local/tomcat/bin
mark@localhost bin % ./startup.sh 
```
如果提示Permission denied:那是因为没有.sh的权限。
```
chmod u+x *.sh
```
再次执行 startup.sh 即可启动服务
```
mark@localhost bin % ./startup.sh 
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /Library/Java/JavaVirtualMachines/jdk1.8.0_301.jdk/Contents/Home
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```
在浏览器中访问 http://localhost:8080 即可看到提示：
### 3. 修改使用的端口号
如果使用的端口号8080不能使用，可通过修改conf文件下的server.xml配置文件来使用其他端口：
![](https://img-blog.csdnimg.cn/20190220171236981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rvbmd6aGVuc29uZw==,size_16,color_FFFFFF,t_70)
**重新启动服务**
```
$ ./shutdown.sh

$ ./startup.sh
```
### 4. 配置Tomcat应用管理GUI用户
打开conf文件夹下的tomcat-users.xml 添加一个用户：
```
<role rolename="manager-gui"/>
<user username="tomcat" password="s3cret" roles="manager-gui"/>
```
![](https://img-blog.csdnimg.cn/20190220174027195.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rvbmd6aGVuc29uZw==,size_16,color_FFFFFF,t_70)
重新启动服务，访问 http://localhost:8090 , 点击Manager App：
用户名与密码即刚设置的 tomcat 与 s3cret
![](https://img-blog.csdnimg.cn/20190220174431882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rvbmd6aGVuc29uZw==,size_16,color_FFFFFF,t_70)