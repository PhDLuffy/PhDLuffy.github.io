---
layout: post
title: Oracle甲骨文云永久免费VPS保活攻略
subtitle: Oracle虚拟机伪CPU内存带宽占用以保活
date: 2023-03-15
author: PhDLuffy
header-img: img/Blog/05.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

Termius登录Oracle甲骨文云虚拟机VPS

Github项目地址[Mrmineduce21/Oracle_OneKey_Active: 为了应对甲骨文最新回收机制而作的垃圾脚本](https://github.com/Mrmineduce21/Oracle_OneKey_Active)



#### 一键吃掉CPU（可精准控制）【OracleLinux不可用】

OneKeyFuck_OCPU.sh

```ssh
cd /root && wget -qO OneKeyFuck_OCPU.sh https://raw.githubusercontent.com/Mrmineduce21/Oracle_OneKey_Active/main/OneKeyFuck_OCPU.sh && chmod +x OneKeyFuck_OCPU.sh && bash OneKeyFuck_OCPU.sh
```

释放CPU资源

```ssh
pid=$(ps -ef | grep "bash" | grep '/bin/bash' | grep -v grep | awk '{print $2}') && kill -9 $pid
```

当ssh终端出现 nohup: appending output to 'nohup.out' 时按回车即可





#### OneKey_FuckMemory.sh 脚本将自动判断内存占用是否到达10%
如果没到达则占用内存至15%

真--一键吃内存

```ssh
cd /root && wget -qO OneKey_FuckMemory.sh https://raw.githubusercontent.com/Mrmineduce21/Oracle_OneKey_Active/main/OneKey_FuckMemory.sh && chmod +x OneKey_FuckMemory.sh && bash OneKey_FuckMemory.sh
```

取消内存消耗(释放内存)

```ssh
cd /root && wget -qO memory_usage.sh https://raw.githubusercontent.com/Mrmineduce21/Oracle_OneKey_Active/main/memory_usage.sh && chmod +x memory_usage.sh && bash memory_usage.sh release
```

#### 带宽占用待更新
