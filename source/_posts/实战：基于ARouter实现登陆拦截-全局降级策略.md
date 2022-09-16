---
layout: post
title: 实战：基于ARouter实现登陆拦截&全局降级策略
toc: true
reward: true
date: 2022-09-16 22:58:36
tags: [Android,实战]
categories: [Android]
---
##  实战：基于ARouter实现登陆拦截&全局降级策略

* 需求分析
* 成果展示
* 疑难点分析
* Coding实现
### 需求分析
  

* * *


  * 利用ARouter拦截页面跳转，实现全局页面降级
<!-- more -->
### Coding
1、引入Arouter组件到项目中
````

android {
    defaultConfig {
        ...
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [AROUTER_MODULE_NAME: project.getName()]
            }
        }
    }
}



dependencies {
    // 替换成最新版本, 需要注意的是api
    // 要与compiler匹配使用，均使用最新版可以保证兼容
    compile 'com.alibaba:arouter-api:x.x.x'
    annotationProcessor 'com.alibaba:arouter-compiler:x.x.x'
    ...
}
// 旧版本gradle插件(< 2.2)，可以使用apt插件，配置方法见文末'其他#4'// Kotlin配置参考文末'其他#5'

```


### 一、功能介绍

1. 支持直接解析标准URL进行跳转，并自动注入参数到目标页面中
2. 支持多模块工程使用
3. 支持添加多个拦截器，自定义拦截顺序
4. 支持依赖注入，可单独作为依赖注入框架使用
5. 支持InstantRun
6. 支持MultiDex(Google方案)
7. 映射关系按组分类、多级管理，按需初始化
8. 支持用户指定全局降级与局部降级策略页面、拦截器、服务等组件均自动注册到框架
9. 支持多种方式配置转场动画
10. 支持获取Fragment
11. 完全支持Kotlin以及混编(配置见文末 其他#5)
12. 支持第三方 App 加固(使用 arouter-register 实现自动注册)
13. 支持生成路由文档
14. 提供 IDE 插件便捷的关联路径和目标类支持增量编译(开启文档生成后无法增量编译)
15. 支持动态注册路由信息

### 二、典型应用
1. 从外部URL映射到内部页面，以及参数传递与解析
2. 跨模块页面跳转，模块间解耦拦
3. 截跳转过程，处理登陆、埋点等逻辑
4. 跨模块API调用，通过控制反转来做组件解耦