---
title: Flutter安装流程
excerpt: 安装流程以及一些特殊情况的处理
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/index_flutter1.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/banner_flutter5.png
tags: [Flutter]
category_bar: true
categories: [Flutter]
date: 2022-11-01 14:07:02
---

## 安装流程

1. 下载解压到指定目录
2. 终端执行
   ``` bash
   $ vim ~/.zshrc
   ```
3. `export PATH="$PATH:【目录】/bin"`
4. 安装 AS，安装 Flutter 插件
5. 安装 VS Code，安装 Flutter 插件
6. 终端执行
   ``` bash
   $ flutter doctor
   ```

## 特殊处理

1. cmd 报错
   打开 AS
   ![](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/08F134DA-2055-4819-AD49-5189CCA48E96.png)
2. **Unable to find bundled Java version**
   [macOS 系统 flutter 配置环境 Unable to find bundled Java version](https://www.jianshu.com/p/aaf181276357)
3. **Try setting CHROME_EXECUTABLE to a Chrome executable**
   `export CHROME_EXECUTABLE="/Applications/Google Chrome Dev.app/Contents/MacOS/Google Chrome Dev"`
4. 切换 Xcode 版本
   ``` bash
   $ sudo xcode-select --switch /Applications/Xcode.app
   ```
5. Xcode Info.plist 配置
   `NSBonjourServices`—`_dartobservatory._tcp`