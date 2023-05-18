---
layout: post
title: X-UI添加节点
subtitle: vless-vmess-trojan-域名下级分页
date: 2023-02-28
author: PhDLuffy
header-img: img/Blog/1.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

### X-UI添加节点

#### vmess节点

![1-vemss设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202302281331307.jpg)

#### Vless节点



![vless节点信息](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202302281332390.jpg)

#### trojan节点



![trojan节点信息](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202302281332743.jpg)

#### 域名下级分页节点

先建立vmess节点

协议选择vmess，路径自定义 `/xxx` tls不要勾选

![3-x-ui端vmess设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202302281343593.jpg)

导入V2rayN

双击编辑

地址为X-ui相同域名，端口号改成443，路径改为域名下级分页`/XXX`底层传输安全选择`tls`跳过证书验证`false`

![2-V2rayN设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202302281343377.jpg)

分享以V2rayN的导出链接为准。