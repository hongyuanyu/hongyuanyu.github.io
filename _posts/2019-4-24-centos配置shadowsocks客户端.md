---
layout:     post
title:      CentOS 7安装配置Shadowsocks客户端
subtitle:   技术分享
date:       2019-4-24
author:     Hongyuan
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - 技术分享
    - 科学上网
---


> Linux


CentOS 7安装配置Shadowsocks客户端
==========================

`Linux`

* * *

1\. 安装配置Shadowsocks客户端
----------------------

### 1.1 安装Shadowsocks客户端

*   安装epel扩展源  
    采用Python包管理工pip安装。

1.  `sudo yum -y install epel-release`
2.  `sudo yum -y install python-pip`

*   安装Shadowsocks客户端

1.  `sudo pip install shadowsocks`

### 1.2 配置Shadowsocks客户端

*   新建配置文件

1.  `sudo mkdir /etc/shadowsocks`
2.  `sudo vi /etc/shadowsocks/shadowsocks.json`

*   添加配置信息

1.  `{`
2.   `"server":"1.1.1.1",`
3.   `"server_port":1035,`
4.   `"local_address":  "127.0.0.1",`
5.   `"local_port":1080,`
6.   `"password":"password",`
7.   `"timeout":300,`
8.   `"method":"aes-256-cfb",`
9.   `"fast_open":  false,`
10.   `"workers":  1`
11.  `}`

参数说明：  
server：Shadowsocks服务器地址  
server_port：Shadowsocks服务器端口  
local_address：本地IP  
local_port：本地端口  
password：Shadowsocks连接密码  
timeout：等待超时时间  
method：加密方式  
workers:工作线程数  
fast\_open：true或false。开启fast\_open以降低延迟，但要求Linux内核在3.7+。开启方法 `echo 3 > /proc/sys/net/ipv4/tcp_fastopen`

*   配置自启动  
    ① 新建启动脚本文件/etc/systemd/system/shadowsocks.service，内容如下：

1.  `[Unit]`
2.  `Description=Shadowsocks`

4.  `[Service]`
5.  `TimeoutStartSec=0`
6.  `ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/shadowsocks.json`

8.  `[Install]`
9.  `WantedBy=multi-user.target`

② 启动Shadowsocks客户端

1.  `systemctl enable shadowsocks.service`
2.  `systemctl start shadowsocks.service`
3.  `systemctl status shadowsocks.service`

*   验证Shadowsocks客户端是否正常运行

1.  `curl --socks5 127.0.0.1:1080 http://httpbin.org/ip`

若Shadowsock客户端已正常运行，则结果如下：

1.  `{`
2.   `"origin":  "x.x.x.x"  #你的Shadowsock服务器IP`
3.  `}`

2\. 安装配置Privoxy
---------------

Shadowsocks是一个 socket5 服务，我们需要使用 Privoxy 把流量转到 http／https 上。

### 2.1 安装Privoxy

*   安装Privoxy

1.  `sudo yum -y install privoxy`

*   启动Privoxy

1.  `systemctl enable privoxy`
2.  `systemctl start privoxy`
3.  `systemctl status privoxy`

### 2.2 配置Privoxy

*   配置Privoxy  
    ① 修改配置文件/etc/privoxy/config

1.  `sudo vi /etc/privoxy/config` 

② 确保如下内容没有被注释掉

1.  `listen-address 127.0.0.1:8118  # 8118 是默认端口，不用改`
2.  `forward-socks5t /  127.0.0.1:1080  .  #转发到本地端口`

*   设置http/https代理  
    ① 修改配置文件/etc/profile

1.  `sudo vi /etc/profile`

添加如下信息：

1.  `export http_proxy=http://127.0.0.1:8118`
2.  `export https_proxy=http://127.0.0.1:8118`
3.  `source /etc/profile`

**注：**端口和privoxy 中的监听端口保持一致

*   验证是否可用

1.  `curl www.google.com`

3\. 参考链接
--------

*   [centos7 安装shadowsocks客户端](http://foxhound.blog.51cto.com/1167932/1969142)
*   [在 CentOS 7 下安装配置 shadowsocks](http://morning.work/page/2015-12/install-shadowsocks-on-centos-7.html)
*   [CentOS 7 安装 shadowsocks 客户端](https://brickyang.github.io/2017/01/14/CentOS-7-%E5%AE%89%E8%A3%85-Shadowsocks-%E5%AE%A2%E6%88%B7%E7%AB%AF/)

