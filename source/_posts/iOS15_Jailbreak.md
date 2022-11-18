---
title: iOS15 越狱
excerpt: iOS 15.0-15.7.1 (semi-)tethered checkm8 "jailbreak"
index_img: https://avatars.githubusercontent.com/u/114239186?s=100&v=4
# banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154334.png
tags: [iPhone]
category_bar: true
categories: [iPhone]
---

## 步骤

### 一、越狱
``` bash
git clone --recursive https://github.com/palera1n/palera1n && cd palera1n

vim palera1n.sh

https://updates.cdn-apple.com/2022FallFCS/fullrestores/012-38914/C7764173-5CC4-4D58-8F8B-F093F9A060F0/iPhone_4.7_P3_15.7_19H12_Restore.ipsw

// 关机后必须使用命令重新引导越狱或移除越狱环境
sudo ./palera1n.sh --tweaks 15.7

// 可以重启，但越狱环境失效且iPhone7&8的Home按钮会失效
// 需要额外的5-10GB储存空间
sudo ./palera1n.sh --tweaks 15.7 --semi-tethered
```

{% note success %}
成功的提示
![成功的提示](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221117113928.png)
{% endnote %}

## 二、特殊情况处理
### 1. `Error init failed`

![Error init failed](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221118142103.png)

{% note secondary %}
**解决方法：**

``` bash
vim palera1n.sh

# 替换以下内容
https://updates.cdn-apple.com/2022FallFCS/fullrestores/012-38914/C7764173-5CC4-4D58-8F8B-F093F9A060F0/iPhone_4.7_P3_15.7_19H12_Restore.ipsw
```

替换位置：
![替换位置](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221118142511.png)
{% endnote %}

### 2. `File exists`

![File exists](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221118142640.png)

{% note secondary %}
**解决方法：**

``` bash
git clone https://github.com/verygenericname/SSHRD_Script --recursive && SSHRD_Script

./sshrd.sh 15.6
./sshrd.sh boot

./sshrd.sh ssh
apfs_deletefs disk0s1s8
```
{% endnote %}
