---
title: Flutter面试
excerpt: 包含了 Dart、Flutter、iOS 的部分知识点
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154309.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154334.png
tags: [Flutter]
category_bar: true
categories: [Flutter]
---

# 大纲
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/xmind_flutter.png)

## Dart部分

### 1. 级联操作符
{% note secondary %}
Dart 当中的 `..` 意思是 **级联操作符**，为了方便配置而使用。
`..` 和 `.` 不同的是 调用 `..` 后返回的相当于是 this，而 `.` 返回的则是该方法返回的值。
``` dart
person..name = '李四'
      ..age = 30
      ..printInfo();
```
{% endnote %}
### 2. `dynamic` ,  `var` , `object` 的区别
{% note secondary %}
`var` 如果有初始值，类型被锁定，而 `dynamic` 和 `object` 都是动态任意类型
`dynamic` 编译阶段不检查类型，调用不存在的方法和变量不会报错
`object` 编译阶段检查类型，调用不存在的方法和变量会报错
{% endnote %}
### 3. `const` 和 `final` 的区别
{% note secondary %}
**相同点：**
1. 类型声明可以省略
``` dart
final String a = 'Ssiswent';
final a = 'Ssiswent';

const String a = 'Ssiswent';
const a = 'Ssiswent';
```

2. 声明时必须要赋值，初始化后不能再赋值
3. 不能和 `var` 同时使用

**不同点：**
1. `const` 需要确定的值，初始值只能是编译时确定的值，比如当前时间
``` dart
final dt = DateTime.now(); ✅

const dt = const DateTime.now(); ❎
```

2. `const` 修饰的 List 集合任意索引不可修改，`final` 修饰的可以修改
``` dart
final List ls = [11, 22, 33];
ls[1] = 44;

// 编译时不报错，运行时提示出错
const List ls = [11, 22, 33];
ls[1] = 44;
```
4. 相同的值，`final` 会在内存中重复创建，`const` 会引用同一份值
{% endnote %}
