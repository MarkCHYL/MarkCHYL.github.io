---
layout: post
title: Java 面试题
toc: true
reward: true
date: 2022-07-23 23:44:54
tags: [基础，面试题]
categories: [Java]
---
[TOC]
### 2、`List`和`Set`的区别
  **List和Set都是继承自Collection接口**
  *  List特点：元素有放入顺序，元素可重复。和数组类似，List可以动态增长，查找元素效率高，但是插入和删除元素的效率低
  *  Set特点： 元素无放入顺序，元素不可以重复，重复元素会被覆盖掉。查找检索元素效率低，但是删除和插入效率高

### 3、HashSet是如何保证元素不重复的？
向`HashSet`中`add()`元素时，判断元素是否存在的依据，不仅要比较`hash`值，同时还要结合`equeals`方法比较。
`HashSet`中的`add()`方法会使用`HashMap`的`add()`方法。
```java
  private static final Object PRESENT = new Object();
  private transient HashMap<E,Object> map;
  public HashSet(){
    map = new HashMap<E, Object>();
  }
  public boolean add(E e){
    return map.put(e,PRESENT)==null;
  }
```