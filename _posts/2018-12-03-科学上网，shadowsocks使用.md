---
layout:     post
title:      科学上网
subtitle:   shadowsocks配置
date:       2018-12-03
author:     Hongyuan
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - 技术分享
    - 原创
---


> 原创笔记

#### 服务端配置

1. 简介：自己搭建服务器的优势是稳定，便宜，任意多设备使用。比直接买VPN比便宜，灵活。

2. 大致流程：首先租一个国内可以访问的服务器，然后在服务器端安装shadowsocks，在国内的设备连接上服务器即可科学上网。

3. 服务器选购，这个环节很关键。如果租的服务器访问不了直接凉凉，目前推荐bandwagon，

租一个一年20美元的服务器，一个月500G流量，可以支付宝。购买之前搜一下搬瓦工优惠码，可以得到6%优惠。

网址：[https://bwh1.net/index.php](https://bwh1.net/index.php)，失效请搜索banwagon或者搬瓦工。

推荐: KVM的，速度快点，OpenVZ支持ipv6。如果流量多，一定要选KVM。

4. 登陆服务器段，下载个xshell，创建一个新的连接，输入IP，端口，账号root，密码自己服务器密码。

5. 服务器一般操作系统是centos，刚买的什么包都没有，需要自己安装下，比如rz sz zip unzip等，安装python安装工具yum install
```
python-setuptools && easy_install pip
```
6. 下载shadowsocks服务器端[https://github.com/shadowsocks/shadowsocks/tree/master](https://github.com/shadowsocks/shadowsocks/tree/master)，可以git也可以本地下载用rz传上去，再unzip解压出来。进入文件夹后输入:
```
python setup.py instal
```
7. 安装好后，修改配置文件：
```
vi /etc/shadowsocks.json
{
    "server":"my_server_ip", //服务器的ip
    "server_port":8388, //本地的端口，可以自己设置，8385，8389什么的
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", //自己设置的密码
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": true
}
```
这里需要使用vim编辑，可以先百度下vim怎么用
8. 加速命令，输入
```
echo 3 > /proc/sys/net/ipv4/tcp_fastopen
```
9. 后台启动
```
ssserver -c /etc/shadowsocks.json -d start
```
10. 后台关闭
```
ssserver -c /etc/shadowsocks.json -d stop
```
#### 客户端配置
1. PC参考这个wiki，[https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients#windows](https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients#windows)
大致界面
![PC客户端界面](https://i.postimg.cc/NjhFXVR6/ss-hongyuan.png)
2. 安卓手机，安装谷歌游戏全家桶。然后下载shadowsocks安卓版本，输入自己服务器的ip和密码端口即可[https://play.gaoogle.com/store/apps/details?id=com.github.shadowsocks](https://play.gaoogle.com/store/apps/details?id=com.github.shadowsocks)
3. 苹果手机和ipad，有很多软件，比如shadowRocket，目前中国市场已经下架了，需要将账号切换到其他国家或者地区。也可以借别人已有这个软件的登陆他的账号下载下，或者万能的淘宝了解一下。
