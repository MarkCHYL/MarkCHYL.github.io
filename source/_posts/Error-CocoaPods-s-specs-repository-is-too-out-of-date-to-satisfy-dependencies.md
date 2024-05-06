---
layout: post
title: 'Error: CocoaPods''s specs repository is too out-of-date to satisfy dependencies'
toc: true
reward: true
date: 2023-05-30 13:59:26
tags: [Error]
categories: [Flutter]
---
1. flutter clean
2. delete /ios/Pods
3. delete /ios/Podfile.lock
4. flutter pub get
5. from inside ios folder: pod install
6. flutter run