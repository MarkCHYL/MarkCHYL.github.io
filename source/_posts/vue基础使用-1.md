---
layout: post
title: vue基础使用-1
toc: true
reward: true
date: 2020-08-25 14:50:27
tags: [vue]
---
### 安装vue
***
[官方文档](https://cn.vuejs.org/v2/guide/installation.html)
我才用的是通过 ***npm*** 进行安装穿件vue项目的，首先切换到项目根目录下，终端先后执行如下：
<!-- more -->
```
npm init //如果你是第一次使用npm的话

npm install --yes //安装后会生成一个package.json的文件

npm install vue //执行完后会出现一个node_modules文件夹，这个便是我们vue.js的资源文件
```
### 如何开始使用
* 1、在body中引包
``` <script type="text/javascript" src="./node_modules/vue/dist/vue.js"></script>```
       
* 2、创建实例化对象
> new Vue({
		el: '#app', //目的地
	    data: data,
		template: ''
	});  
   
        
[本节练习代码]()

下一章[vue基础使用-2](http://markchyl.cn/2020/08/25/vue基础使用-2/)