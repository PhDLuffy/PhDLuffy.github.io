---
layout:     post
title:      黑群晖Docker青龙面板薅京东羊毛
subtitle:   青龙面板
date:       2022-04-07
author:     PhDLuffy
header-img: img/Blog/blog-2022-04-07-01.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧
---



### 准备

自带ShadowSocksR Plus+的Openwrt系统路由器

黑群晖自带Docker

### 安装

#### 在docker目录下新建ql文件夹，ql文件夹下面新建以下文件夹

> 通过ssh命令直接建立docker总是出错，应该是文件夹权限的问题，所以提前在相应文件夹下建立以下文件夹，以防出错

![新建docker挂载文件夹](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406155355458.png)

#### 打开Termius或其他ssh工具，连接黑群晖的ssh，安装青龙面板Docker

先获取管理员权限

```json
sudo -i
```

运行以下命令，直接复制粘贴到Termius即可，粘贴快捷键为Control+Shift+v

```json
docker run -dit \
  -v $PWD/ql/config:/ql/config \
  -v $PWD/ql/log:/ql/log \
  -v $PWD/ql/db:/ql/db \
  -v $PWD/ql/repo:/ql/repo \
  -v $PWD/ql/raw:/ql/raw \
  -v $PWD/ql/scripts:/ql/scripts \
  -p 5700:5700 \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:2.10.13
```

运行完成之后，打开DSM的Docker管理界面，会发现青龙面板没有运行，会出错，我们点击容器-编辑

将系统默认的文件夹都删除，重新添加我们上面自己建立的文件夹，保存，重新启动docker容器，这时候应该就正常启动了

> 系统默认的文件夹路径应该是root/ql/XXXX，我们都改为自己建立的文件夹，比如docker/ql/XXXXX

![替换系统默认添加的文件夹](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406160138031.png)

#### 青龙面板设置

网页URL输入你的群晖ip:5700

通知方式跳过，我们之后再重新设置

![通知方式跳过](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406160431555.png)

用户名和密码记好，一会登录要用，忘记了可以通过ssh查询用户名和密码

```json
docker exec -it qinglong bash
cat /ql/config/auth.json
```



![用户名和密码设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406160504961.png)

进入之后，环境变量，新建变量，名称必须是JD_COOKIE，值的获取下面讲解

![环境变量](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406160706746.png)

##### 获取京东Cookie

打开[京豆网址](https://bean.m.jd.com/)

登录之后F12

**点击【Network】--【ALL】 搜索【log】刷新页面 在cookie中找到pt_key=xxxxxxxxxxxxxxx;pt_pin=xxxxxxxxx;**

![chrome的F12调试模式获取cookie](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/1.png)

##### 输入cookie到青龙面板

名称：JD_COOKIE

值：刚才复制下来的pt_key=xxxx;pt_pin=xxxx;

![JD_COOKIE](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/1%20(1).png)

##### 添加定时任务

大佬的脚本仓库，这里是我自己fork之后再引入，我直接输入大佬的网址出错，应该是大佬的网址默认的ma1n不是真正的main分支

定时规则为`0 0 * * *`

```json
ql repo https://ghproxy.com/https://github.com/shufflewzc/faker2.git "jd_|jx_|gua_|jddj_|getJDCookie" "activity|backUp" "^jd[^_]|USER|function|utils|sendNotify|ZooFaker_Necklace.js|JDJRValidator_|sign_graphics_validate|ql"
```

![定时任务设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406162609836.png)

添加之后，运行一下，在openwrt全局代理的情况下，会刷出大佬的很多定时脚本，如果没有openwrt全局代理的路由，只会出现网络连接失败。

#### 添加Telegram通知

新建TG机器人

在Telegarm中搜索`@BotFather`

输入`/newbot`\

输入机器人自定义名字，比如PhDLuffy青龙机器人

输入机器人自定义ID，PhDLuffyql_bot，这时会获得Token，记下来备用



在Telegarm中搜索`@PhDLuffyql_bot`，点击自己建立的机器人，点击开始

> 如果不打开自己的机器人点击开始的话，后面添加会出现400错误



在Telegarm中搜索`@getuseridbot`

点击开始，会获得账号的id

##### 青龙面板设置

![系统设置-通知设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406163331719.png)



通知方式选择Telegram

输入我们在telegram中获得的Token和UserID

![Token和UserId](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406163412499.png)

##### 点击保存，这里如果上面我们在telegram中自己的机器人中点击了开始，这里就会保存成功，否则显示400错误。



青龙控制面板，配置文件，找到第3项Telegram，双引号中填写Token和UserID，点击保存

![双引号中填写Token和UserID](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220406163640028.png)



#### 添加企业微信通知

在企业微信群中添加群机器人

![image-20220407103307519](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220407103307519.png)



给机器人起个名字，确定，获得的web_hook地址后面的key值复制下来

青龙控制面板，配置文件，找到第5项企业微信机器人，双引号中填写key的值，点击保存

![企业微信机器人QYWX_KEY](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220407103639528.png)



在定时任务找到**京东资产变动通知**这个定时任务，运行一下，telegram和企业微信的机器人账号会弹出通知。

以上。

