---
layout: post
title: 'error: <item> inner element must either be a resource reference or empty.'
toc: false
reward: false
date: 2019-12-25 15:47:08
tags: [Error]
categories: [Android]
---
```
FAQ:

Android resource compilation failed
Output: /home/cmm/code/AndroidHttpCapture/app/build/intermediates/incremental/mergeDebugResources/merged.dir/values/values.xml:733: error: <item> inner element must either be a resource reference or empty.

Command: /home/cmm/.gradle/caches/transforms-1/files-1.1/aapt2-3.2.0-4818971-linux.jar/8e0275d065e63c2601af4fb5800833ab/aapt2-3.2.0-4818971-linux/aapt2 compile --legacy \
-o \
/home/cmm/code/AndroidHttpCapture/app/build/intermediates/res/merged/debug \
/home/cmm/code/AndroidHttpCapture/app/build/intermediates/incremental/mergeDebugResources/merged.dir/values/values.xml
Daemon: AAPT2 aapt2-3.2.0-4818971-linux Daemon #1
```


如上错误原来是values.xm资源文件中，元素定义了id后，就不能在后面给值了


发现 :
`<item name="split" type="id">false</item>`

改为 :
`<item name="split" type="id"/>`
即可.