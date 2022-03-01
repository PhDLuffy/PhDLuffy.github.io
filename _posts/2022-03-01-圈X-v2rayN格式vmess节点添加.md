---
layout:     post
title:      quantumultX中v2rayN格式vmess节点添加
subtitle:   quantumultx系列
date:       2022-01-01
author:     PhDLuffy
header-img: img/post-index-bg-dengziqi.jpg
music-id: 1454317928
catalog: true
tags:
    - 软件



---



vmess节点v2rayN格式

vmess://XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

直接复制进入圈x的**URI**进行单节点添加



<img src="https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220301193044184.png" alt="image-20220301193044184" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220301193027457.png" alt="image-20220301193027457" style="zoom:50%;" />

由于目前圈x下v2ray的vmess协议有默认ahead=true，而旧服务器搭建的v2ray还没有更新这个新特性，所以要在圈X中改为false

在设置底部，点击编辑，进入json代码编辑，选择右上角箭头，跳转到server_local，在vmess地址上，将aead=true改为aead=false



<img src="https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220301193318745.png" alt="image-20220301193318745" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220301193350181.png" alt="image-20220301193350181" style="zoom:50%;" />

这样，节点上的A字符就会消失，可以正常上外网了。



<img src="https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220301200300706.png" alt="image-20220301200300706" style="zoom:50%;" />

