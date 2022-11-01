---
title: Flutter面试
excerpt: 包含了 Dart、Flutter、iOS 的部分知识点
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154309.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154334.png
tags: [Flutter]
category_bar: true
categories: [Flutter]
date: 2022-11-01 15:22:08
---

# 大纲
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/xmind_flutter.png)

## Dart部分

1. **级联**操作符
> Dart 当中的 「..」意思是 「级联操作符」，为了方便配置而使用。「..」和「.」不同的是 调用「..」后返回的相当于是 this，而「.」返回的则是该方法返回的值 。
``` dart
person..name = '李四'
      ..age = 30
      ..printInfo();
```