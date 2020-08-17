---
layout: post
title: Mark的个人博客搭建
date: 2018-12-14 20:30:47
toc: true
reward: true
tags: [Hexo,工具搭建]
---
***
# **标题: Mark的个人博客搭建**
***
### **一、前言**
> 大家好，我是Mark，一个Android程序员，目前从事软件应用开发，其实我很早就像想很多的大佬们一样拥有自己的博客，在下在网上搜了一波，发现可以通过 GitHub + Hexo 搭建自己个人的博客，这次我就想彻底好好的把这个弄好，主要是可以方便自己把自己的笔记记录在上面，方便自己的以后的只是复习。我的脑子不太好使，记东西过了不久就会忘记，好记性不容烂笔头，对于一个程序员如果还在用笔记本自己手写笔记确实让我自己感到惭愧，程序的代码量和知识点很多，若是每个自己的笔记都去用手写，那样我的工作效率就会很低。在此我想大家推荐，
<!--more-->
### **二、记录下我搭建此博客方法**
利用GitHub + Hexo 搭建个人的博客，网上有很多的大佬的文章，这里我想大家推荐一个博客文章[JackyRoc的文章链接](https://www.cnblogs.com/jackyroc/p/7681938.html)，至于怎么利用Hexo在下推荐先看看此文章[小茗同学的博客园](https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html).

好了！现在开始记录我自己的搭建步骤：

#### 首先：我们的搭建环境是 MacBook Pro，准备的编写博客的编辑器 **[Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432)**,**GitHub**，以及重要的安装好 **git**，再就是很重要的是耐心和百折不挠的信息。Start！！！

   ### 1. 创建仓库（https://markchyl.github.io/）
   仓库的名字是这个格式 XXXXX.github.io，XXXXX 就是你的自己可以定义的，后面的 .github.io 一定要这样，当然如果你有自己的个人域名地址也是可以在后面的步骤中绑定的。创建仓库的步骤我就不啰嗦了！简单。

   ### 2. Hexo的安装
   要使用Hexo,需要安装Nodejs以及Git，
   [参考:安装Node.js](http://www.runoob.com/nodejs/nodejs-install-setup.html)

   在你需要安装Hexo的目录下(新建一个文件夹)右键选择 Git Bash

npm install hexo-cli -g   
hexo init #初始化网站   
npm install   
hexo g #生成或 hexo generate   
hexo s #启动本地服务器 或者 hexo server,这一步之后就可以通过http://localhost:4000  查看了
详细命令请参考Hexo文档

这里介绍一下怎么创建一篇文章

hexo new "文章名" #新建文章
hexo new page "页面名" #新建页面   
常用简写

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
新建一篇文章后就可以预览了,在hexo new之后执行一次生成hexo g再执行hexo s启动本地服务器,如果之前还在hexo s 按Ctrl + C 结束.


### 3.添加主题
安装主题(yilia主题):
hexo clean
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia   
启动主题
找到目录下的_config.yml 文件,打开找到 theme：属性并设置为yilia

更新主题
cd themes/yilia
git pull
hexo g
hexo s
此时刷新http://localhost:4000/页面就能看到新的主题了.

### 4. 部署到Github
1.检查SSH keys的设置

以下命令均是在Git bash里输入
```
cd ~/.ssh
ls
```
#此时会显示一些文件
```
mkdir key_backup
cp id_rsa* key_backup
rm id_rsa* 
```

#以上三步为备份和移除原来的SSH key设置
> ssh-keygen -t rsa -C "邮件地址@youremail.com" #生成新的key文件,

**邮箱地址填你的Github地址**
***
#Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
#接下来会让你输入密码
之后就可以看到成功的界面。
***

2.添加SSH Key到Github
进入github首页在设置洁面
![](https://images2017.cnblogs.com/blog/1250458/201710/1250458-20171017153636849-294935886.png)
***
#### 添加SSH Key
![](https://images2017.cnblogs.com/blog/1250458/201710/1250458-20171017153642521-647884655.png)
找到系统当前用户目录下(开启查看隐藏文件) C:\Users\用户名\ .ssh id_rsa.pub文件以文本方式打开。打开之后全部复制到key中
![](https://images2017.cnblogs.com/blog/1250458/201710/1250458-20171017153648631-441574444.png)
到了这就可以测试一下是否成功了:

ssh -T git@github.com
#之后会要你输入yes/no,输入yes就好了。
设置你的账号信息:

git config --global user.name "你的名字"     #真实名字不是github用户名
git config --global user.email "邮箱@邮箱.com"    #github邮箱
3.部署到github
hexo d
这时再刷新 username.github.io 就可以看到你的博客了。

到了这你以为就结束了吗？没有，还有坑没有给你们填好。


如果你的SSH配置的时候哪找的.ssh文件的化，可以参照[mac 上传本地代码到 Github 教程](https://www.cnblogs.com/ailiailan/p/8577411.html),我就是参照上面的步骤配置的。


