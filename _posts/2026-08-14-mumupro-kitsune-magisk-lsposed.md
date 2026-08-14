---
layout: post
title: 又能愉快的在电脑上刷抖音了
subtitle: mac上使用mumupro安卓模拟器+狐狸面具+LSPosed模块
date: 2026-08-14
author: PhDLuffy
header-img: img/header-img/8/14.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

## 前记

在M1 macmini上安装了mumupro，

用来刷抖音好几年了，

今年开始突然就不稳定了，

动不动就卡死，

运行其他的软件没什么问题，

一刷抖音就容易卡崩溃。

所以是时候与时俱进升级一波<刷抖音流>了。

## 准备

软件如下：

mumupro，v1.4.11

kitsune，v27.2-kitsune-2，sha256:d4499b53b7b6b2528123cc352b6008671ec1d9d71d621a608e74699708514197

> AI说v27.2-kitsune-2，这个第二版本最稳定

LSPosed，v2.1.1-7790

> 需要magisk版本至少为v27

抖音，v39.4.0，内部代号39409900

Dyyds，v260630153221（127）

> 支持抖音最新版本为39409900，即v39.4.0

Root Explorer，v4.12.6

软件提前放入mumupro的共享文件夹中，

以便在模拟器中使用

## 开搞

### mac安装mumupro模拟器及配置

新增新的安卓模拟器，

注意，

选择**可写系统盘**和**开启手机root权限**，

这两点非常重要。

![image-20260814141408676](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141414818.png)

![image-20260814141431933](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141414976.png)

![image-20260814141445688](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141414745.png)

![image-20260814141533110](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141415143.png)

![image-20260814141518047](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141415089.png)

### 狐狸面具安装

打开模拟器，

双击安装狐狸面具，

初次安装会提醒请求root权限，

给予通过即可。

安装完成后打开，

选择安装，

会发现只有一个**选择并修补一个文件**的安装方式，

上划清除app，

重新打开狐狸面具，

再次选择安装，

此时会出现三个安装方式，

选择第三个**直接安装（直接修改/system）**，

安装完成之后，

选择重启。

### 删除mumupro模拟器的root管理文件

打开狐狸面具，

此时会提醒异常状态，

![image-20260814142402170](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141424211.png)

需要手动删除mumupro自带的root文件

安装root explorer，

删除`/system/app`目录下的`SuperUser`文件夹

删除`/system/xbin`目录下的`su`文件

### 狐狸面具配置

打开狐狸面具

点击右上角齿轮，

在Magisk选项中，

勾选Zygisk

![image-20260814142217041](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141422084.png)

再次重启

点击模块，从本地安装，选择LSPosed的zip文件，

安装完成重启。

再次进入狐狸面具，

选择模块，点击LSPosed模块下面的`Action`，即可进入LSPosed的管理界面

### 安装抖音和dyyds模块

双击安装抖音apk，

双击安装dyyds模块，

在狐狸面具的LSPosed模块下的`Action`跳转LSPosed模块管理开启和查看抖音模块。

点击启用即可。

### 抖音设置

进入抖音，

首先是版权规避同意，打字输入我承诺blabla一堆，点击同意。

在我的，设置，第一行即是模块设置，自行设置并导出配置备份到本地。

以上完成。

![image-20260814143514225](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202608141435273.png)
