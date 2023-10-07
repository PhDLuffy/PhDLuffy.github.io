---
layout: post
title: frp的后备选择zerotier(更新)
subtitle: zerotier自建planet根服务
date: 2023-10-07
author: PhDLuffy
header-img: img/Blog/09.jpg
music-id: 1399335349
catalog: true
tags:
  - 软件
---

参考链接：[搭建私有的planet-github](https://github.com/Jonnyan404/zerotier-planet/issues/11#issuecomment-1059961262)

前置操作：[[2023-08-13-zerotier-planet-docker|vps安装docker, docker-compose]]

### 下载

建立docker文件夹, 

在docker文件夹下操作以下代码

```json
git clone https://github.com/Jonnyan404/zerotier-planet
cd zerotier-planet
vim docker-compose.yml
```

### 修改

编辑docker-compose文件

- 修改MYADDR为自己vps服务器ip，

- 修改ZTNCUI_PASSWD网页管理密码

```

```json
### date:2021年11月29日
### author: www.mrdoc.fun | jonnyan404
### 转载请保留来源
### update：2022年08月14日
version: '2.0'
services:
    ztncui:
        container_name: ztncui
        restart: always
        environment:
            - MYADDR=xxx.xxx.xxx.xxx #改成自己的服务器公网IP
            - HTTP_PORT=4000
            - HTTP_ALL_INTERFACES=yes
            - ZTNCUI_PASSWD=web管理登录密码
        ports:
            - '4000:4000' # web控制台入口
            - '9993:9993'
            - '9993:9993/udp'
        volumes:
            - './zerotier-one:/var/lib/zerotier-one'
            - './ztncui/etc:/opt/key-networks/ztncui/etc'
            # 按实际路径挂载卷， 冒号前面是宿主机的， 支持相对路径
        image: keynetworks/ztncui

```

### 运行

```json
docker-compose up -d

docker images #　查看镜像
docker container ps -a # 查看容器

docker exec -it ztncui bash # 进入容器
# 在容器内操作
cd /var/lib/zerotier-one
ls -l
# 生成moon配置文件
zerotier-idtool initmoon identity.public > moon.json
chmod 777 moon.json
```

### 自定义planet

新建一个terminal, 在**容器外**修改`moon.json`, 位置对应挂载位置

修改`stableEndpoints`, 注意格式和实际公网ip

```json
{
 "id": "b72b5e9e1a",
 "objtype": "world",
 "roots": [
  {
   "identity": "b72b5e9e1a:0:a892e51d2ef94ef941e4c499af01fbc2903f7ad2fd53e9370f9ac6260c2f5d2484fd90756bec0c410675a81b7cf61d2bb885783bd6a8c28bce83bcab5f03fe14",
   "stableEndpoints": ["127.0.0.1/9993"]   ### 修改这一行后面ip及端口，ip为vps的ip，端口号为9993
  }
 ],
 "signingKey": "45f0613e569a0549c74293c39b30495b594a003534290e8ade9ef82877aa7505d7a73eeabfc22c97c404e4caaf9f3c9eed2b134d696935c966e28f523364f15f",
 "signingKey_SECRET": "cc6afd67e7b7f84a92e2c8d3c2e7212c71e2ad0a4f5b3c03bf60ab1cd3b99281b57d9a2958d2bd8fc2bc77fdf2a1160099c2c61d3d9acc8cb311673ee120b4a6",
 "updatesMustBeSignedBy": "45f0613e569a0549c74293c39b30495b594a003534290e8ade9ef82877aa7505d7a73eeabfc22c97c404e4caaf9f3c9eed2b134d696935c966e28f523364f15f",
 "worldType": "moon"
}
```

在**容器内**生成moon文件, 此命令在docker容器内运行

```json
zerotier-idtool genmoon moon.json
mkdir moons.d
cp *.moon moons.d/
```

在**容器外**生成planet文件

- 拷贝一份moon文件， 客户端可以用到，可在vps的挂载对应此容器的文件夹拷贝moon文件，为一串数字.moon文件。
- 下载[mkmoonworld](https://github.com/kaaass/ZeroTierOne/releases/tag/mkmoonworld-1.0), 拷贝moon.json， 放在一个目录下

```json
./mkmoonworld-x86_64 ./moon.json
mv world.bin planet
```

```json
# 复制到容器内
docker cp ./planet ztncui:/var/lib/zerotier-one
```

重启容器

```json
docker restart ztncui
docker exec -it ztncui bash # 进入容器
# 在容器内操作
cd /var/lib/zerotier-one
#　查看ｍoon
zerotier-cli listmoons
```

访问`ip+端口`对应的设置页面

替换客户端的planet文件并重启服务， 再加入网络， 在网页端授权

> 客户端需要同时替换planet文件和moon文件

以上。
