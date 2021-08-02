---
layout: post
title: 如何修改表结构：ALERT语句
toc: true
reward: true
date: 2021-04-19 16:32:02
tags: [Oracle]
categories: [数据库]
---
应用场景：当我们的业务需要对已经存在的表进行表结构修改时，我们需要使用 ALERT 语句进行修改。
### 增加表列:
```
ALERT TABLE tablenamme ADD
(
    column dataType [DEFAULT expr][NOT NULL],
    column dataType [DEFAULT expr][NOT NULL],
    ....
);
```
### 修改表列:
```
ALERT TABLE tablenamme MODIFY
(
    column dataType [DEFAULT expr][NOT NULL],
    column dataType [DEFAULT expr][NOT NULL],
    ....
);
```
**注意事项**：
* 修改表列时，可以增加 `NOT NULL`及默认值约束，不能增加其他约束
* 增加的默认值只会影响到后来插入的值
* 若是要缩小列的宽度时，只有表中该列为空后者表里没有记录
* 如若这列有空值，则不能添加非空约束
* 若是改变该列的类型，只有表中该列为空后者表里没有记录

### 删除表中的列
```ALERT TABLE tablenamme DROP column col_name;```
### 增加约束
```
ALERT TABLE tablenamme 
ADD [CONSTRAINT constraint] type (column);
```
* 增加 `NOT NULL`约束只能在增加列和修改列的时候添加