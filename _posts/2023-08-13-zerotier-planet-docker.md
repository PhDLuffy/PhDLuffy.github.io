---
layout: post
title: frp的后备选择zerotier
subtitle: zerotier自建planet异地局域网
date: 2023-08-13
author: PhDLuffy
header-img: img/Blog/09.jpg
music-id: 5260494
catalog: true
tags:
    - 软件

---

垃圾腾讯云的轻量服务器到期了，从50块一年涨价到500块一年，先用低价吸引人，然后很多人因为迁移麻烦就选择了高价续费，这种营销策略真是恶心至极，要不是因为是国内ip，谁会选择垃圾腾讯云？

Oracle和Cloudcone的两个常年用的服务器物美价廉，除了是墙外ip，连接速度没有国内ip快，其他都非常完美。

这次觉得把zerotier的docker部署在Cloudcone上。

## 在ubuntu上安装docker

Termius连接cloudcone服务器(ubuntu 20.04 LTS系统)

更新软件列表

`apt-get update`

安装前置程序

`apt-get install ca-certificates curl gnupg`

安装 GPG key

`install -m 0755 -d /etc/apt/keyrings`

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`

`chmod a+r /etc/apt/keyrings/docker.gpg`

设置仓库

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

安装docker

`apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

安装docker-compose

`apt-get install docker-compose`

验证docker，docker-compose是否安装成功

`docker --version`

`docker-compose --version`

## 安装zerotier的docker版本

在服务器`~`目录下新建docker文件夹

`mkdir docker`

cd到docker目录下

`cd docker`

下载源代码

`git clone https://github.com/Jonnyan404/zerotier-planet`

cd到代码文件夹

`cd zerotier-planet`

vi修改docker-compose.yml文件，修改MYADDR为自己服务器的公网ip

启动容器

`docker-compose up -d`

创建planet和moon

```bash
# 以下步骤为创建planet和moon
docker cp mkmoonworld-x86_64 ztncui:/tmp
docker cp patch.sh ztncui:/tmp
docker exec -it ztncui bash /tmp/patch.sh
docker restart ztncui
```

然后浏览器访问 `http://ip:4000` 打开web控制台界面。

新建一个用户，再用新建用户登录，再删除admin用户

配置网络，参考之前的配置。

浏览器访问 `http://ip:3180` 打开planet和moon文件下载页面（亦可在项目根目录的`./ztncui/etc/myfs/`里获取）。

下载下来的planet文件非常重要。

## 配置客户端zerotier

### windows客户端配置

官网下载zerotier客户端

将 `planet` 文件覆盖粘贴到 `C:\ProgramData\ZeroTier\One` 中(这个目录是个隐藏目录，需要运允许查看隐藏目录才行)
Win+S 搜索 服务

![找到服务](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202308131810440.png)

找到ZeroTier One，并且重启服务

![重启zerotier服务](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202308131811040.png)

使用管理员身份打开PowerShell
执行如下命令，看到join ok字样就成功了

```bash
PS C:\Windows\system32> zerotier-cli.bat join 网络id(就是在网页里面创建的那个网络)
200 join OK
PS C:\Windows\system32>

```

登录管理后台可以看到有个个新的客户端，勾选Authorized就行

![勾选认证](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202308131812645.png)

执行如下命令

```bash
PS C:\Windows\system32> zerotier-cli.bat peers
200 peers
<ztaddr>   <ver>  <role> <lat> <link> <lastTX> <lastRX> <path>
fcbaeb9b6c 1.8.7  PLANET    52 DIRECT 16       8994     1.1.1.1/9993
fe92971aad 1.8.7  LEAF      14 DIRECT -1       4150     2.2.2.2/9993
PS C:\Windows\system32>

```

可以看到有一个 PLANTET 和 LEAF 角色，连接方式均为 DIRECT(直连)
到这里就加入网络成功了

### Mac客户端配置

步骤如下：

1. 进入 `/Library/Application\ Support/ZeroTier/One/` 目录，并替换目录下的 `planet` 文件
2. 重启 ZeroTier-One：`cat /Library/Application\ Support/ZeroTier/One/zerotier-one.pid | sudo xargs kill`
3. 加入网络 `zerotier-cli join` 网络 `id`
4. 管理后台同意加入请求
5. `zerotier-cli peers` 可以看到` planet` 角色

### 安卓客户端配置

暂缓

### NAS配置

同样替换planet文件

![docker的zerotier根目录下](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202308131818679.png)

查看连接状况

`docker exec -it 容器id zerotier-cli status`

加入网络，记得去网站ui授权设备

`docker exec -it 容器id zerotier-cli join 网络id`

查看已加入网络

`docker exec -it 容器id zerotier-cli listnetworks`

离开已加入网络

`docker exec -it 容器id zerotier-cli leave 网络id`
