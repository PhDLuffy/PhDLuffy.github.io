---
layout: post
title: scrutiny硬盘温度监控
subtitle: 时刻监控硬盘温度，数据无价啊无价
date: 2025-10-06
author: PhDLuffy
header-img: img/header-img/10/6.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

## docker安装Scrutiny

新建目录`scrutiny`

新建文件`docker-compose.yml`

```bash
version: '3.5'

services:
  scrutiny:
    container_name: scrutiny
    image: ghcr.io/analogj/scrutiny:master-omnibus
    cap_add:
      - SYS_RAWIO   # 获取机械硬盘的S.M.A.R.T 信息，默认即可
      - SYS_ADMIN    # 获取NVMe硬盘的S.M.A.R.T 信息，没有可以删除
    ports:
      - "8183:8080" # webapp
      - "8184:8086" # influxDB admin（可以不映射）
    volumes:
      - /run/udev:/run/udev:ro
      - ./config:/opt/scrutiny/config
      - ./influxdb:/opt/scrutiny/influxdb
    devices:
      - "/dev/sda"
      - "/dev/sdb"
      - "/dev/nvme0n1"

```

新建目录`config`和`influxdb`

目录`/config`新建文件`collector.yaml`

```bash
devices:
  - device: /dev/sda
    type: 'sat'
  - device: /dev/sdb
    type: 'sat'
  - device: /dev/nvme0n1
    type: 'nvme'
```

Termius进入ssh

目录`./scrutiny`执行

```bash
docker-compose up -d
```

以上。

参考文献：

[Docker 部署 Scrutiny 监控你的硬盘 - 猫猫博客](https://catcat.blog/docker-install-scrutiny-monitor-harddrive.html)





