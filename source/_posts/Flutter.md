---
title: Flutter基本组件
excerpt: 各个基本组件的介绍和使用
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/Flutter.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/banner_flutter5.png
tags: [Flutter]
category_bar: true
categories: [Flutter]
date: 2022-10-25 17:07:02
---

# 基本组件

## Container和Text
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/4C9D9D95-AA12-47E2-BC3B-0270252495BA.png)

## Image
1. 第一种
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/4102977A-A7BC-493A-86B7-1E04B656B4F4.png)

2. 第二种
> 利用Container的DecorationImage参数实现  
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/D4303067-4A38-45CE-96A5-B3DD2F84484C.png)

3. 第三种
> 利用ClipOval组件实现自动裁剪的圆形图片(可能为椭圆)  
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/3F00E607-419A-418D-9A6E-393C56193ECA.png)

## 加载本地图片
1. 创建对应文件夹，导入图片
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/3178FB08-173A-487E-94EA-26BFBDEBA6F9.png)

2. 编辑pubspec.yaml文件
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/EB57CA1F-3F5D-4DBD-8CEF-EA9B829498E6.png)

3. 使用
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/A17CED54-40B2-4C71-9A4D-ACA45937D229.png)

## ListView
1. 简单图文列表
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/8913CD23-D36A-48F5-8951-A12B02FD19FE.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/25A188DC-54D9-439C-ACA8-60513EE5AF2E.png)

2. 水平列表
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/58D63904-5020-4E49-8FBF-4F552B145160.png)

3. 模拟动态获取列表
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/6B97628D-848F-4E67-BF68-11082D8E12F8.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/61F9AF85-5289-4594-B1E0-5B3B8C2F39A8.png)

4. builder方法
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/97813325-A959-43E9-9F1F-129756C351A6.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/9B31A6F4-59E2-4085-8069-476FF36F75ED.png)

## GridView
1. GridView.count
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/183BA66F-6A14-4E58-8DF3-0AE98DCDDF7A.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/0963AD1D-D872-434A-9BA9-3502676B0052.png)

2. GridView.builder
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/B732685D-A4EC-4BF9-8559-A195B233C7FC.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/94FA71E0-DF86-4C73-9FF3-8B92DD42BEBF.png)

## Padding
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/63A718DE-4170-4F7D-BA4D-A852A3A217A1.png)

## Row和Column
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/81244562-478E-4C9F-8315-1803AFD83F9D.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/36F7BAB4-E614-456C-8AFE-6089DE66FEAF.png)

## Expanded
1. 比例分配
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/423EF451-95ED-464F-978B-F02B870292B0.png)

2. 左侧固定宽度，右侧自适应宽度
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/F56DD291-C2AA-4563-BD65-7214237F805A.png)

## Stack(层叠，指定位置)
1. 直接使用
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/43D25948-91BC-4BFF-92A1-199176B01549.png)

2. 结合Align组件
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/7EE640EA-F91C-4E4B-B19E-F07C1843CB3A.png)

3. 结合Positioned组件
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/29C256E1-16CD-4F60-B14F-F80A29A58C3D.png)

## AspectRatio和Card
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/407142B7-745D-47BA-A3F7-26C786AD664F.png)

## Wrap(搜索筛选等，自动换行布局)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/8D3615D5-8D1A-419D-B498-469FE5C170AF.png)

## StatefulWidget
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/844BC555-1684-434C-BF18-3FD19E05DB9A.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/998022BC-957E-432F-A40A-9D5DA747F111.png)

## BottomNavigationBar（Tab）
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/81D0AA2E-DAF4-42EC-A473-1FFAC13104FF.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/7ACCEA44-19F2-4BAA-AA6C-55EFCB392BCA.png)

## 普通路由、普通路由传值
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/78A09833-2DBF-4A0D-ACA1-9713B0B2B710.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/72DF56DC-94AF-4634-A334-BCAC968624E9.png)

## 命名路由、命名路由传值
### 命名路由
1. 在main.dart中配置
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/B991015B-D67A-488F-BA9F-A78394BC817D.png)

2. 调用
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/549A2995-B3A3-49E5-8186-A69B5C07A8F3.png)

### 命名路由传值
1. 方式一：
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/111ED87C-E9BD-46D9-9291-FBF2C8674D94.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/58CC4A42-E08D-4CA8-9A91-0438D3CDC713.png)

2. 方式二：
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/0F6EFDDB-C8D2-46AD-8B52-428DB7DB1DC7.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/8FD7088C-92C5-48CC-9A0A-08B1681B4C81.png)
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/0886B706-BA88-4023-A8FA-F81C58DE255A.png)

3. 如果是StatefulWidget
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/FBD24BE8-0B72-4F4D-860C-E96ECC4C84BB.png)

## 路由替换
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/97C21CD6-DBD2-47A3-B46A-D8F0F2E0809F.png)

## 返回到指定路由
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/A0C6DF6A-A5DC-40F0-89AC-1C82BA3E0E19.png)


