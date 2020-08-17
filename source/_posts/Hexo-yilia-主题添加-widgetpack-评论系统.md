---
layout: post
title: Hexo yilia 主题添加 widgetpack 评论系统
date: 2019-07-21 22:24:41
toc: true
reward: true
tags: [Hexo,工具搭建]
---
## 前言：
---
> 记得这周都在忙着给我的博客添加评论功能，脑子都懵逼啦！开始起初是想利用GitHub的gitment和gittalk进行集成，按照的网上的套路也弄了，总是报错，对于我只是安卓原生的小码农来说，我完全抓瞎。
<!--more-->
**下面的分析来自**[阿甘的博客](https://blog.csdn.net/ganzhilin520/article/details/79048010 )

目前博客站点使用的评论功能，多说，网易云跟贴都已经下线。Disqus也被挡在墙外，友言貌似也不行。

可用的评论系统大概有：  
- HyperComments：https://www.hypercomments.com （来自俄罗斯的评论系统，使用谷歌账号注册。可以访问，不会用，好气，，）

- 来必力：https://livere.com （来自韩国，使用邮箱注册。）

- 畅言： http://changyan.kuaizhan.com （安装需要备案号。不太好用。）

- Gitment： https://github.com/imsun/gitment （有点小bug，比如说每次需要手动初始化，登录时会跳到主页。。）

- Valine: https://github.com/xCss/Valine (基于Leancloud的极简风评论系统，用了下，没效果，是我Next主题的原因还是？）

综上，我开始采用了来必力。
**可能是来必力来自韩国，我没调查资料，是看上面的文字时韩国的瞎说的，功能我到是实现了，界面渲染有点卡顿，最终放弃了。

# widgetpack 评论系统
这是我这次笔记的重点，此刻是周末晚上，明早就要上班啦！给自己定的任务计划没完成，Mark我有点睡不着，无聊的我打开了电脑发现了这个**widgetpack**，在此感谢博主的贡献[貌似掉线](https://blog.csdn.net/maosidiaoxian/article/details/94651023),

- 免费的
- 哪儿的？国外的，放心没被强，我是科学守法的好公民
- 还没调查，貌似是欧洲的，[介绍地址](https://widgetpack.com/comment-system)
- 邮箱注册便可

下面是[貌似掉线](https://blog.csdn.net/maosidiaoxian/article/details/94651023)的原文：
## 集成步骤：
### 1. 主题配置添加 widgetpack
修改 hexo 博客目录的 theme/yilia 中的 _config.yml 文件，增加如下配置：
```
# widgetpack。将 false 改为 widgetpack 上的 id 则启用该评论系统。
widgetpack: false
```

### 2. 新增 widgetpack 代码文件
在 yilia 中的 layout/_partial/post 下新增 widgetpack.ejs文件，内容如下：
这段代码，在你注册 widgetpack 之后也会有
```
<div id="wpac-comment"></div>
<script type="text/javascript">
wpac_init = window.wpac_init || [];
wpac_init.push({widget: 'Comment', id: <%=theme.widgetpack%>});
(function() {
    if ('WIDGETPACK_LOADED' in window) return;
    WIDGETPACK_LOADED = true;
    var mc = document.createElement('script');
    mc.type = 'text/javascript';
    mc.async = true;
    mc.src = 'https://embed.widgetpack.com/widget.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(mc, s.nextSibling);
})();
</script>
<a href="https://widgetpack.com" class="wpac-cr">Comments System WIDGET PACK</a>

```
### 3. 修改 article.ejs
由于我使用的 yilia 主题没有自带 widgetpack 的代码，修改 yilia 中的 layout/_partial/article.ejs 文件，在 <% if (!index && post.comments){ %> 后的任意一个评论代码前或后插入如下代码：
```
    <%if (theme.widgetpack) { %>
      <%- partial('post/widgetpack') %>
    <% } %>
```
这段代码加在哪儿？打开之后好好的看编辑器上的代码。

----
**如上三步，修改完成。如果要启用，修改主题的 _config.yml 文件，将 widgetpack 的值改为 widgetpack 上的 id 即可，注意冒号之后有空格**