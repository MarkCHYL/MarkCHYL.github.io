---
layout: post
title: android 修改项目包名详解
date: 2019-08-06 23:00:42
tags: [Android]
---
再简单的东西也的记下来，不然容易忘记。
此处原文地址来源：https://www.jianshu.com/p/1891d5a1a121
>在android 平常项目开发中，修改项目包名是很常见的事，哪如何有限修改包名一步到位呢？经过几次痛苦的经历后，觉得有必要记录一番！
 <!--more-->
对于修改包名，一般有两种情况：
1）一个是包名目录结构不变，比如说，将包名“com.zlc.xuexi”，改成"com.xuexi.zlc"
2）另一个是包名目录结构改变了，目录级数改变了，比如说，从"com.xuexi.zlc"，改变成"com.xuexi.zlc.zlc"，这里包名的目录结构就从3级改变成为了4级
下面分别来讲解一下这两种情况
1、第一种情况：包名目录结构不变
针对第一种情况，其实特别好改，步骤截图如下：
切换的Progject结构，查看java包名结构，一般是这样的

![](https://upload-images.jianshu.io/upload_images/2108792-a0f6b84a3fcd681d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507/format/webp)

点击show options menu按钮

![](https://upload-images.jianshu.io/upload_images/2108792-8e9f64f76dd666e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/534/format/webp)
***

![](https://upload-images.jianshu.io/upload_images/2108792-ca64b8ff27c0e2f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/350/format/webp)

去掉勾上的 Hide Empty Middle Packages  和 Show Members

![](https://upload-images.jianshu.io/upload_images/2108792-245a095cfef4c8c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/293/format/webp)

java包的展示目录结构就改变了

![](https://upload-images.jianshu.io/upload_images/2108792-b188356d6a454df0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/538/format/webp)

对于包名目录结构不改变的。就分别改各个层次对应的包名或者直接按快捷键 Shift + F6

![](https://upload-images.jianshu.io/upload_images/2108792-5748e03a357ac8d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/747/format/webp)
***
![](https://upload-images.jianshu.io/upload_images/2108792-bbf9f6e3517de803.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/629/format/webp)

同理，假如是3级目录结构包名，每一个都要改变的话，就按照上图的做法一个个更改
接着，去改app模块下的build.gradle文件

![](https://upload-images.jianshu.io/upload_images/2108792-c2b0a1b594ceb7a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/531/format/webp)
***

![](https://upload-images.jianshu.io/upload_images/2108792-b50f2f39318e3a86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/651/format/webp)

然后去修改AndroidManifest.xml文件

![](https://upload-images.jianshu.io/upload_images/2108792-f7573970e99b92ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/672/format/webp)

最后，点击sync同步一下就大功告成了
2、第二种情况：包名目录结构要改变的
针对第二种情况，步骤截图如下：

![](https://upload-images.jianshu.io/upload_images/2108792-a0f6b84a3fcd681d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507/format/webp)
点击show options menu按钮

![](https://upload-images.jianshu.io/upload_images/2108792-8e9f64f76dd666e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/534/format/webp)

***
![](https://upload-images.jianshu.io/upload_images/2108792-ca64b8ff27c0e2f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/350/format/webp)

去掉勾上的 Hide Empty Middle Packages  和 Show Members

![](https://upload-images.jianshu.io/upload_images/2108792-245a095cfef4c8c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/293/format/webp)

java包的展示目录结构就改变了

![](https://upload-images.jianshu.io/upload_images/2108792-b188356d6a454df0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/538/format/webp)

对于包名目录结构不改变的。就分别改各个层次对应的包名或者直接按快捷键 Shift + F6

![](https://upload-images.jianshu.io/upload_images/2108792-5748e03a357ac8d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/747/format/webp)
***
![](https://upload-images.jianshu.io/upload_images/2108792-bbf9f6e3517de803.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/629/format/webp)

假如是3级目录结构包名，改成4级包名目录机构，首先要新建包然后去移动其他的目录包

![](https://upload-images.jianshu.io/upload_images/2108792-e43fefa7fb413d3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/692/format/webp)

新建好多一级的目录包之后，需要移动启动文件夹到该目录包下

![](https://upload-images.jianshu.io/upload_images/2108792-33d2a624c533d767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/635/format/webp)

接着，去改app模块下的build.gradle文件

![](https://upload-images.jianshu.io/upload_images/2108792-c2b0a1b594ceb7a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/531/format/webp)
***

![](https://upload-images.jianshu.io/upload_images/2108792-b50f2f39318e3a86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/651/format/webp)

然后去修改AndroidManifest.xml文件

![](https://upload-images.jianshu.io/upload_images/2108792-f7573970e99b92ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/672/format/webp)

最后，点击sync同步一下就大功告成了
3、注意
如果项目上用了DataBinding框架，特别是第二种情况，恭喜你，你肯能有得忙了。项目上有DataBinding框架的时候，当你按照上面的步骤修改了包名，就会报一个这样的错

![](https://upload-images.jianshu.io/upload_images/2108792-534897b4744d5df5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)
***

![](https://upload-images.jianshu.io/upload_images/2108792-feb5f5023ca50e8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

遇到这个情况，肯定是修改包名或者移动了包名结构，但是布局文件或者java文件的的dataBinding的引用没有改变

![](https://upload-images.jianshu.io/upload_images/2108792-9a32a6931bffa51b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/631/format/webp)

***
![](https://upload-images.jianshu.io/upload_images/2108792-849db258b30d1aeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/650/format/webp)

这里我没找到特别快速修改的方法，放在我是一个个去检查java文件的导包和xml布局文件的应用，看对不对，不对就要手动改过来了，呜呜。。。。。。

![](https://upload-images.jianshu.io/upload_images/2108792-73e253453e85f5a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/538/format/webp)

如果，确定全部改完无误之后，重新Rebuild Project

![](https://upload-images.jianshu.io/upload_images/2108792-8f5d3f454854f75c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/519/format/webp)

万一，还是有刚才那个错误的话，记得再回头检查一遍java文件和布局文件，看看各自的引用对不对，如果全部都改对之后，还是有错误的话，哪就静下心来错误提示

![](https://upload-images.jianshu.io/upload_images/2108792-77579a031568c1a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

如果不是DataBinding引起的话，一般都会找到比较明显的提示

==========我是有分割线的：2019.04.15更新=================================
如果真不好遇到第二种情况的话，面对databanding这种框架，那就只能使用全局替换的方法了
ctrl + Shift + R
