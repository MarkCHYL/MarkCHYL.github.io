---
layout: post
title: MAC搭建基于RTMP的本地Nginx服务器
date: 2018-12-17 11:02:21
toc: true
reward: true
tags: [Android,视频直播]
---
### MAC搭建基于RTMP的本地Nginx服务器，实现电脑上视频推流。
# 1、先安装homeView
<!--more-->
```
//安装命令
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

//移除命令
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

# 2、安装Nginx服务器
### 增加对nginx的扩展;也就是从github上下载,home-brew对ngixnx的扩展
### homebrew/nginx的git路径变了(貌似是2018年3月更新)
```
 brew tap homebrew/nginx
```
# 3、安装Nginx服务器和rtmp模块
```
brew install nginx-full --with-rtmp-module
```
![image](https://upload-images.jianshu.io/upload_images/1027569-1480f7cb0be838e2.png?imageMogr2/auto-orient/)

# 4、查看nginx的信息
```
brew info nginx-full
```
### 结果显示
![](https://upload-images.jianshu.io/upload_images/1027569-da39e4efd3bf35f4.jpeg?imageMogr2/auto-orient/)

```
nginx的安装位置
/usr/local/Cellar/nginx-full/1.10.1/bin/nginx
nginx配置文件所在位置
/usr/local/etc/nginx/nginx.conf
nginx服务器根目录所在位置 
 /usr/local/var/www
```
#### 在浏览器中输入 [http://localhost:8080 ](http://localhost:8080 )（若是安装成功就会出现如下图所示）

![](https://upload-images.jianshu.io/upload_images/1027569-8f0712d428c40368.png?imageMogr2/auto-orient  "安装成功")

# 5、配置rtmp和支持http协议拉流
#### 在终端中输入
```
open /usr/local/etc/nginx
```
#### 打开niginx的文件夹，找到nginx.conf文件,用xcode打开。添加下面配置
```
#在http节点下面(也就是文件的尾部)加上rtmp配置：
rtmp {//协议名称
   server {//说明内部中是服务器相关配置
        listen 1992;// 监听的端口号, rtmp协议的默认端口号是1935
        application Mark {//访问的应用路径是 Mark
              live on; //开启实时
              record off;// 不记录数据
        }
    }
}

location /hls {
        #Serve HLS config
        types {
            application/vnd.apple.mpegurl    m3u8;
            video/mp2t ts;
        }
        root /usr/local/var/www;
        add_header Cache-Control    no-cache;
    }
```
#  6、保存文件后，重新加载nginx的配置文件
```
nginx -s reload
```
#### 配置好的样子如下：
![](https://upload-images.jianshu.io/upload_images/1027569-3a79f914d42877ae.png?imageMogr2/auto-orient/)

# 7、安装ffmepg工具
```
brew install ffmpeg
```
# 8、通过ffmepg命令进行推流测试
-  ####  推流至RTMP到服务器
     
    -  ####   生成地址： rtmp://localhost:1992/Mark/room
```
ffmpeg -re -i 你的视频文件的绝对路径(如/Users/lideshan/Downloads/test.mp4) -vcodec copy -f flv rtmp://localhost:1992/Mark/room//
 如：我把测试视频放在桌面
ffmpeg -re -i  /Users/Mark/Desktop/test.mp4 -vcodec copy -f flv rtmp://localhost:1992/Mark/room

```
#### 这里Mark是上面的配置文件中,配置的应用的路径名称;后面的room可以随便写

-   #### 推流至HLS到服务器
    - 生成地址: http://localhost:8080/hls/test.m3u8
    
```
    ffmpeg -re -i /Users/apple/Desktop/ffmepg/story.mp4 -vcodec libx264 -vprofile baseline -acodec aac -ar 44100 -strict -2 -ac 1 -f flv -s 1280x720 -q 10 rtmp://localhost:1935/hls/demo
```