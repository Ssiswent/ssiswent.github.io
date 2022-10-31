---
title: 导出证书教程
excerpt: 用于签名ipa文件
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/apple_developer.png
# banner_img: https://developer.apple.com/news/images/og/programs-og.jpg
tags: [iPhone]
date: 2022-10-31 17:07:02
---

1. 打开 [开发者账号管理网站](https://developer.apple.com/account/resources/certificates/list)

2. 添加UDID
(获取方式：[三分米](http://udid.sanfenmi.cn))
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/67352D15-60BB-496C-988E-72B7C62EBA00.png)

3. 创建Certificates
选择**iOS Distribution**
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/03AD3613-EEA9-47E8-9CED-59E77CF75979.png)
下载Certificate并双击打开，在钥匙串中右键导出p12证书

4. 创建ID
勾选推送权限
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/7E21C08F-77B4-4F59-A921-9F1E4C49D2A1.png)

5. 创建Profiles
选择**Ad Hoc**
选择对应ID、Certificates
勾选对应Device
![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/1EE5D61E-BC6C-4A4C-AD70-C23EB8040B44.png)
下载Profile