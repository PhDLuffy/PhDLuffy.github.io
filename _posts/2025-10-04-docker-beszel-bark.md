---
layout: post
title: 通过docker安装beszel监控主机性能
subtitle: 一定要监控温度，all in one，all in boom
date: 2025-10-04
author: PhDLuffy
header-img: img/header-img/10/4.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧

---

## 搭建中心(hub)

在远端具有公网ip的主机上搭建中心(hub)

docker目录下建立`beszel`文件夹，

在`beszel`文件夹下，同时新建`beszel_data`和`beszel_socket`文件夹。

新建`docker-compose.yml`文件

```bash
version: '3.3'

services:
  beszel:
    image: henrygd/beszel:latest        
    container_name: beszel              
    restart: always                     
    ports:
      - 8090:8090                       
    volumes:
      - ./beszel_data:/beszel_data      
      - ./beszel_socket:/beszel_socket  

```

启动容器

```bash
docker-compose up -d
```

主机上的`frpc`服务，将`8090`端口暴露到公网域名

使用公网域名打开beszel服务

新建管理员

新增客户端agent

![image-1757386430536](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202510042251911.png)



名称：主机名，自定义

主机/IP：必填，可填真实ip或者瞎编一个

端口：固定为45876

公钥和令牌自动生成

点击复制`docker-compose`文件，例如`文件D`



## 客户端搭建代理(agent)

在本地需要监控的主机上搭建代理(agent)

docker目录下建立`beszel-agent`文件夹，

在`beszel-agent`文件夹下，新建`beszel_agent_data`文件夹。

将上一步生成的`文件D`复制到`beszel-agent`文件夹下

```bash
version: '3.3'

services:
  beszel-agent:
    image: henrygd/beszel-agent                 # 使用 henrygd/beszel-agent 镜像
    container_name: beszel-agent                # 容器名称为 beszel-agent
    restart: always                             # 容器异常退出时自动重启
    network_mode: host                          # 使用宿主机网络模式，方便访问本机端口和资源
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro   # 挂载宿主机 Docker 套接字（只读），用于获取容器运行状态
      - ./beszel_agent_data:/var/lib/beszel-agent      # 将数据持久化到宿主机 ./beszel_agent_data 目录
    environment:
      LISTEN: 45876                           # agent 监听的端口（默认 45876）
      KEY:                                    # 与 hub 配置的 KEY 保持一致，用于安全通信
      TOKEN:                                  # 可选的认证 Token，用于加强安全性
      HUB_URL: http://localhost:8090          # hub 服务的访问地址（例如 http://<hub服务器IP>:8090）本机可以使用http://localhost:8090,其他主机请填相应的ip
```

KEY和TOKEN为服务器自动生成，

HUB_URL：可以填入反代的https域名



## beszel网页端管理

此时再次查看网页端，可以看到客户端各性能监控信息

选择设置，通知，Webhook/推送通知

可以通过bark进行服务器性能通知到手机等设备通知

bark的格式为

```bash
bark://:接收通知设备的barkid@自建服务器域名和端口/
```

设备id前面记得有个`:`

bark自建服务器不需要填写`https://`，末尾记得添加`/`

以上



[Docker部署一个轻量易用的服务器监控-Beszel](https://www.ywsj365.com/archives/docker-bu-shu-yi-ge-qing-liang-yi-yong-de-fu-wu-qi-jian-kong--beszel)