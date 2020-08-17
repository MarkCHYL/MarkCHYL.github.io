---
layout: post
title: ' Description : Could not find the specified file.  Suggestion : Check that the path you have specified is correct.'
toc: true
reward: true
date: 2019-12-27 10:08:06
tags: [Error]
---
记录下我使用 CornerStone 利用SVN 进行版本代码管理。我已经快两个月没提交代码啦，今天一提交发现好多问题，各种报错。
譬如这类错误：
```
 Description : Could not find the specified file.  Suggestion : Check that the path you have specified is correct.   Technical Information =====================        Error : V4FileNotFoundError   Exception : ZSVNNoSuchEntryException  Causal Information 
 ```
 解决办法：
 我在missing选项中删除错误的就OK了，提交前全选 =>删除，重新手动编辑提交，重新提交前执行一下  clean。