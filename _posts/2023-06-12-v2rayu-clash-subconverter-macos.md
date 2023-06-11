---
layout: post
title: 再见V2rayU，你好Clash
subtitle: MacOS下从V2rayU转到Clash
date: 2023-06-12
author: PhDLuffy
header-img: img/Blog/06.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

V2rayU已经1年多没更新了，原作者也跑路了，是时候换个工具了

Clash小猫咪很有名，也很好用，是基于网络配置文件的，想迁移一下还是有点麻烦的。

1。安装subconverter的docker应用到DSM

安装地址: [subconverter/README-docker.md at master · tindy2013/subconverter · GitHub](https://github.com/tindy2013/subconverter/blob/master/README-docker.md)

直接命令安装



```json
docker run -d --restart=always -p 25500:25500 tindy2013/subconverter:latest
```



安装完成之后在dsm的ssh中验证安装

```json
curl http://localhost:25500/version
```



2。转换地址

```json
http://【DSM的ip】:25500/sub?target=【clash】&url=【v2rayN中的共享地址】
```



DSM的ip即docker容器所在ip，端口号25500

target选择clash即导出为clash配置连接格式

url选择v2rayN中的共享url



得到的网页内容，另存为yaml，放入clash--配置--配置文件夹

3。在clash中选择生成的配置即可





