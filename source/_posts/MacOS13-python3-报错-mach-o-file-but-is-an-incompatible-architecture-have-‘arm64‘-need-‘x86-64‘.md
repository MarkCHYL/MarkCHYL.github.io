---
layout: post
title: >-
  MacOS13 python3 报错 mach-o file, but is an incompatible architecture (have
  ‘arm64‘, need ‘x86_64‘)
toc: true
reward: true
date: 2023-02-21 14:19:29
tags: [Error]
categories: [Python]
---

解决方案：
前面加上​​​arch -x86_64​​​ 例如：
​​arch -x86_64 pip3 install Pillow​​​​arch -x86_64 pip3 install numpy​​
