---
layout:     post
title:      Openwrt旁路由桥接模式设置
subtitle:   Openwrt
date:       2022-04-01
author:     PhDLuffy
header-img: img/Blog/blog-20220-04-01-1.jpg
music-id: 29802490
catalog: true
tags:
    - 科技生活



---

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# Openwrt旁路由桥接模式设置



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**

### 问题背景：

目前有两个路由器，分别是小米4A千兆版和斐讯K2

小米4A千兆版刷的Openwrt，斐讯K2刷的老毛子固件。

需求是

小米4A千兆版的两个千兆LAN口用于NUC11和DSM黑群晖，充分利用双千兆进行局域网文件共享，

百兆WAN口用于连接斐讯K2的LAN口进行外网传输，同时斐讯K2的LAN口添加PS3，

利用旁路由桥接设置，可使NUC11和PS3处于同一网段通过FileZilla进行FTP通讯，用于拷贝上传ISO文件。



关闭小米4A千兆版的无线功能仅使用有线功能，启用斐讯K2的无线功能用于连接智能家居，这样有线传输和无限传输的任务分开，有利于传输稳定性。

### 设置方法

#### 网络拓扑图

![网络拓扑图](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/未命名文件.png)

网络拓扑图如上，斐讯K2做主路由，无线用于连接智能家居和无限设备，有线端口连接PS3；

小米4A千兆版做旁路由，WAN口连接主路由LAN口，两个千兆LAN口连接DSM黑群晖和NUC11，黑裙通过USB连接打印机

#### 网页设置

##### 主路由斐讯K2网页设置

设置内网LAN，内网设置

IP地址: 192.168.123.1

DHCP设置：启用DHCP服务器，起始地址192.168.123.2，结束地址192.168.123.244

> 这里的IP地址非常重要，这个地址是主路由的访问地址，同时也是网关地址和DNS地址

![DHCP服务器设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220401180610555.png)

无线2.4G和无线5G设置就按照正常的无线设置即可，填写好SSID和密码即可。



##### 旁路由小米4A千兆版网页设置

网络，接口

WAN和WAN6，进入修改页面，高级设置，关闭开机自动运行。

![WAN和WAN6设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220401175912236.png)

接口总览，关闭WAN和WAN6

![关闭WAN和WAN6](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220401175939979.png)

修改LAN

协议选择静态地址

IPv4地址：192.168.123.2

> 这里的ip一定要和主路由在同一个网段，即前三位相同都为192.168.123.X，最后一位随便填，最好填写2，这样我们的旁路由的网络管理页面地址就是192.168.123.2

IPv4网关：192.168.123.1

> 网关地址，我们填写主路由的ip

DHCP基本设置，忽略此接口勾选上，即不在此接口提供DHCP服务，这样是避免旁路由和主路由的DHCP服务冲突。

物理设置，桥接接口勾选上，接口把以太网适配器WAN和WAN6勾选上

![勾选WAN和WAN6](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220401180358661.png)

将旁路由的2.4g和5g wifi关掉，因为我只想用主路由的wifi

保存&应用即可。



以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~~~~~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)
