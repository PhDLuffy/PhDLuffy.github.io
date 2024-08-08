---
layout: post
title: Ollama终于用上我的显卡坞老古董RX580了
subtitle: Ollama在Windows下利用RX580显卡加速并局域网分享
date: 2024-08-08
author: PhDLuffy
header-img: img/Blog/13.jpg
music-id: 29802490
catalog: true
tags:
  - 软件
---

项目地址：[likelovewant/ollama-for-amd](https://github.com/likelovewant/ollama-for-amd)

## 软件准备：

[ROCm Support](https://github.com/likelovewant/ollama-for-amd/wiki)

下载RX580的pre-built ROCm libraries：

[RX580下载gfx803的rocblas](https://github.com/likelovewant/ROCmLibs-for-gfx1103-AMD780M-APU/releases/tag/v0.5.7)  

下载文件：[rocblas.for.gfx803.override.with.vega10.7z](https://github.com/likelovewant/ROCmLibs-for-gfx1103-AMD780M-APU/releases/download/v0.5.7/rocblas.for.gfx803.override.with.vega10.7z)

Ollama-for-amd

下载文件：[0.3.2版本](https://github.com/likelovewant/ollama-for-amd/releases/tag/v0.3.2)

## 安装：

Windows下右键管理员安装Ollama-for-amd

安装完成之后，解压rocblas.for.gfx803，

找到Win下Ollama软件位置

`C:\Users\usrname\AppData\Local\Programs\Ollama\rocm`

替换rocblas.dll文件，替换library文件夹。

## 局域网设置：

[在 Windows 上使用 Ollama 配置本地及外网访问_ollama3 能联网么-CSDN博客](https://blog.csdn.net/weixin_45131680/article/details/138520336)

`Windows+R`快捷键打开[运行]对话框，输入命令：`sysdm.cpl`

系统属性->环境变量->用户变量，新增变量

变量名：OLLAMA_HOST

变量值：`:11434`

> 记得11434前面有个`:`

重启Ollama，局域网网页打开`ip:11434`

如果成功，会显示`Ollama is running`

以上。
