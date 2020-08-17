---
layout: post
title: 如何打Jar工具包
toc: true
reward: true
date: 2019-12-10 16:00:11
tags: [Android]
---
**打开Moudle包的build文件添加如下代码**
```
task makeJar(type: Copy) {
    def filename = projects.name + '-V'
    delete "build/libs/MarkZXing1.0.jar"
    from('build/intermediates/packaged-classes/release')
    into('build/libs/')
    include('classes.jar')
    rename('classes.jar', 'MarkZXing1.0.jar')
}
makeJar.dependsOn(build)
```
**点开我们右侧的Gradle，找到项目下的第一步新建的模块Module名称 ，点开 Tasks/other 文件 找到 makeJar， 双击即可，等待出现BUILD SUCCESSFUL， Task execution finished ‘makeJar’.==》编译完成，此时去Module目录下的libs/下找到的test.jar，便是制作的jar包。**