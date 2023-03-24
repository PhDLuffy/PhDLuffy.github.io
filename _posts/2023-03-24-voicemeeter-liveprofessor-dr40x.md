---
layout: post
title: VoiceMeeter基本设置
subtitle: 背景音乐闪避效果配置
date: 2023-03-24
author: PhDLuffy
header-img: img/Blog/06.jpg
music-id: 26137335
catalog: true
tags:
    - 软件

---


### 准备工作

DR40x安装windows的ASIO驱动

#### HARDWARE OUT

A1：接入DR-X Seies ASIO Driver，用于开启hardware input2的ASIO设备

A2：接入副显示器的音频

A3：接入Airpods

#### HARDWARE INPUT

HARDWARE INPUT：ASIO input(1 + 2)



### 设备声音流

DR40X所在硬件输入

下方输出到B2



背景音乐，通过EarTrumpet，输出到VoiceMeeter Input

> 即Voicemeeter VAIO，也就是第一个虚拟输入，虚拟播放器1
>
> 输出不用选，直接B2调用7,8通道接收背景音乐的1,2通道



B2(虚拟麦克风2,即VoiceMeeter AUX Output)的第1,2通道接收硬件输入(DR40X)的麦克风双通道去1,2通道

B2的第7,8通道接收VoiceMeeter Input (Voicemeeter VAIO)的1,2通道



B2通过AUX ASIO设备进入机架进行声音处理

机架处理完声音从AUX ASIO设备输出到VoiceMeeter AUX Input

> 即Voicemeeter AUX VAIO，也就是第二个虚拟输入，虚拟播放器2
>
> 输出要选
>
> 勾选B1(即VoiceMeeter Output)，其他程序需要选择麦克风输入的地方就可以使用B1(虚拟麦克风1)，此时声音已经是渲染之后有效果的声音
>
> 勾选A3，用于耳机监听





补充：



![VoiceMeeter Banana整体设置图](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303242329768.png)







![LiveProfessor的ASIO设备设置，选择AUX虚拟ASIO](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303242314720.png)



![VoiceMeeter系统设置system settings，注意PATCH COMPOSITE也就是B2的7,8通道接收了虚拟输入1的1,2通道](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303242331854.png)