---
layout: post
title: 上传本地代码到指定的Git仓库
date: 2019-05-29 11:05:46
toc: true
reward: true
tags: [工具使用]
categories: [Git]
---

### 1、本地安装git环境
下载安装包安装即可，在这里不加记录。
***
### 2、初始化git项目，生成 .git 配置目录
进入项目根目录,右键 git bash here打开控制台 ，输入git init即可完成。
 <!--more-->
***
### 3、将项目加入本地git仓库
git add . （此处add后面有空格 和点号）
git status
touch README.md （可不要
git add README.md （可不要
git commit -m "first commit"
***
### 4、在git、码云建好云端项目，生成git url
建好项目，在项目的克隆下载处复制url即可
***
### 5、连接云端仓库,将本地仓库代码提交到云端仓库
连接云端仓库
*git remote add origin https://gitee.com/xxx/xxx.git
*git push origin master
为解决本地与云端版本冲突，加上-f参数，push文件
git push --set-upstream origin master -f
之后会提示输入云端仓库的用户名，密码，验证成功开始上传并完成，实测码云可通过。*
***
不加f会提示错误：
```
! [rejected] master -> master (non-fast-forward)
error: failed to push some refs to 'https://gitee.com/xxx/xxx.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```


到此就完成了目的。
以后每次提交是先提交到本地仓库，需重新运行 
```
git push --set-upstream origin master -f
```
更新到云端
或是`commit `后 `push`
git 修改用户名，邮箱：
当前 project：
直接修改 .git目录下 config文件的
```
[user]
name =xxx
email = xxx
```
或
```
git config user.name 用户名;
git config user.email 邮箱;
```
全局：
```
git config  --global user.name 用户名；
git config  --global user.email 邮箱名;
```
### 6、`git` 删除 项目`remote`
终端执行`git remote remove origin`,origin为项目`remote`的名字

添加远程仓库(远程仓库引用)命令：
> 使用命令：**git remove add 远程仓库到本地的名称  远程仓库的路径**
[更多指令学习参考](https://blog.csdn.net/weixin_44703358/article/details/100514085)