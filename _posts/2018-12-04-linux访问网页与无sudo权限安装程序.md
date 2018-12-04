---
layout:     post
title:      linux访问网页
subtitle:   无sudo权限
date:       2018-12-04
author:     Hongyuan
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - 技术分享
    - 原创
---


> 原创笔记

## 简介

VPN办公时需要提交集群任务，但是在外网无法访问内网比较麻烦。这里可以先通过连上公司的服务器，再在服务器安装linux访问网页的工具即可。lynx是一个基于文本的web浏览器，使用GNU GPLv2协议发布，用ISO C编写。下载地址：[http://lynx.invisible-island.net/release/](http://lynx.invisible-island.net/release/)

## 无sudo权限安装方法

```
解压后
cd package
./configure --prefix=$HOME
make
make install
```

## 使用方法

lynx + 网页地址

比如：lynx [https://www.jianshu.com/u/0f0fd65df64a](https://www.jianshu.com/u/0f0fd65df64a)

可以使用键盘上的上下箭头键进行浏览。在超链接上按下右箭头会打开它，按下左箭头会返回到上一页面，按q键退出。