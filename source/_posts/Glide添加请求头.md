---
layout: post
title: Glide添加请求头
toc: true
reward: false
date: 2020-08-31 16:07:59
tags: [Android,工具使用]
categories: [Android]
---
[原文来自](https://www.jianshu.com/p/a425b5ca89dd)
```
 GlideUrl glideUrl = new GlideUrl(url, new Headers() {
            @Override
            public Map<String, String> getHeaders() {
                Map<String, String> header = new HashMap<>();
                //不一定都要添加，具体看原站的请求信息
                header.put("Referer", "http://www.baidu.com");
                return header;
            }
        });

       Glide.with(context).load(url).into(imageView);
```