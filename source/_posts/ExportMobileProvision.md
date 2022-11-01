---
title: 导出证书教程
excerpt: 用于签名ipa文件
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/apple_developer.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/banner_developer4.png
tags: [iPhone]
category_bar: true
categories: [iPhone]
date: 2022-10-31 17:07:02
---

## 第一步
打开 [开发者账号管理网站](https://developer.apple.com/account/resources/certificates/list)

## 第二步
**添加 UDID**
(获取方式：[三分米](http://udid.sanfenmi.cn))
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/67352D15-60BB-496C-988E-72B7C62EBA00.png)

## 第三步
**创建 Certificates**
选择 `iOS Distribution`
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/03AD3613-EEA9-47E8-9CED-59E77CF75979.png)
下载Certificate并双击打开，在钥匙串中右键导出p12证书

## 第四步
**创建 ID**
勾选推送等必要权限
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/7E21C08F-77B4-4F59-A921-9F1E4C49D2A1.png)

## 第五步
**创建 Profiles**
选择 `Ad Hoc`
选择对应 ID、Certificates
勾选对应 Device
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/1EE5D61E-BC6C-4A4C-AD70-C23EB8040B44.png)
下载 Profile