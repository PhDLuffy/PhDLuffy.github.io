---
layout: post
title: 如何像Mac上的Homebrew一样在Win上安装软件
subtitle: scoop的安装与使用
date: 2023-07-31
author: PhDLuffy
header-img: img/Blog/02.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---



[📦 Windows 一條命令安裝卸載軟件，Scoop 的安裝和基本使用。#Windows / #Scoop_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1dY411G7cT/?spm_id_from=333.880.my_history.page.click&vd_source=ae43e31fa8b0a49f93dbb7dc012860f8)

# 官网安装scoop

[Scoop](https://scoop.sh/)官网

- 打开win的powershell
- `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`更改系统权限，输入`Y`同意
- `irm get.scoop.sh | iex`下载安装
- 安装完成之后，修改代理设置，以clash for window为例，`scoop config proxy 127.0.0.1:7890`

# scoop安装软件

- 检查本地仓库列表是否存在所需软件`scoop search 软件名`
- 添加相应的外部仓库bucket，例如添加extras，`scoop bucket add extras`
- 成功添加之后，`scoop search 软件名`可找到本地仓库名单存在此软件
- `scoop install 软件名`安装即可，例如安装win下好用的磁盘文件占用检查软件wiztree
- `scoop install wiztree`一键安装，干净便捷
