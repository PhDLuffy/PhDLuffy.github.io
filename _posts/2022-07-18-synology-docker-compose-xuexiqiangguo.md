---
layout:     post
title:      群晖Docker安装容器之docker-compose方法
subtitle:   学而时习之，不亦乐乎
date:       2022-07-18
author:     PhDLuffy
header-img: img/Blog/post-index-bg-dengziqi.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧
---



## 群晖Docker安装容器之docker-compose方法



ssh登录docker

`sudo -i`

`cd /volume1`

`cd /docker`



```json
查看本地docker-compose版本

docker-compose version
```



在docker文件夹下建立`容器名Name`文件夹



在`容器名Name`文件夹下新建docker-compose.yml配置文件



```json
version: "3.6"

services:
    techxuexi:
        image: techxuexi/techxuexi-amd64
        container_name: techxuexi-wechat
        restart: always
        volumes:
            - ./xuexi-wechat/user:/xuexi/user
        shm_size: 2gb
        ports:
            - 8088:8088
        environment:
            - Pushmode=2
            - ZhuanXiang=True
            - Scheme=dtxuexi://appclient/page/study_feeds?url=

```

`image`: 映像名

`container_name`: 容器名

`restart`: 自动重启

`volumes`: 映射文件夹，`./`表示本目录，目录下的文件夹提前在`docker/容器名Name`下建立好

`:`后为docker内挂载路径

`shm_size:` 内存配置

`ports`: 端口映射，docker外部端口和内部端口，docker外部端口可通过frp服务映射到公网，公网端口可自定义

`environment`: 环境变量，可定义不同的环境变量



> Pushmode=2的微信公众号的docker端口是8088
>
> Pushmode=6的网页的docker端口是80



运行容器:

```json
docker-compose up -d
```



## frc端口映射

ssh登录docker

`sudo -i`

`ls`

`cd /frp`

编辑`frpc.ini`

添加公网端口映射局域网端口

## 微信配置

微信登录[微信公众号号测试账号申请](https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)

扫码登陆后，系统将会分配一个测试公众号，如下图

![image](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202207181105182.png)

下拉到`测试号二维码`，扫码关注公众号，**并给公众号发任意消息**

成功后将看到你的微信号

![image](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202207181105601.png)

在`/xuexi-wechat/user/settings.conf`中添加微信配置

```json
addition {
   wechat{
       appid = "第2步中获取的appid"
       appsecret = "第2步中获取的appsecret"
       openid = "第4步中获取的微信号"
   }
}
```

完成以上配置，微信就可以给你发送消息了，如果想跟微信互动，请继续执行下面步骤

### 微信进阶设置

以下操作适用于有公网 IP，且需要跟微信互动的用户

修改上面第五步中的配置文件追加`token`属性。token 用于验证请求，随意填写 8~16 位字符即可，也可以到[这里](https://www.toolzl.com/tools/createString.html)生成，*不要勾选字符*

```json
addition {
   wechat{
       appid = "第2步中获取的appid"
       appsecret = "第2步中获取的appsecret"
       openid = "第4步中获取的微信号"
       token = "随机字符串"
   }
}
```

在【接口配置信息】中填入`URL`和第 1 步的`token`。

![image](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202207181105582.png)

URL：`http://你的域名或IP地址:端口号/wechat`

容器的内部端口是 `8088` 请映射到本地端口，并开启防火墙。

> 端口号】是你从 docker 中映射出来的本地端口号，记得在路由器或防火墙中开启端口转发。访问链大概为：`微信--端口A-->路由--端口B-->docker--端口8088-->程序`。 这里要填写的就是`端口A`。

这里的端口号，是公网端口号，frp内网穿透出来的公网端口号



点【提交】，如果提示`token验证失败`,要么端口不通，要么 docker 没启动监听，就不用往下看了。

端口不通：检查 Docker 映射的内部端口是不是 8088，检查本地端口是否被占用，检查防火墙是否放行本地端口

监听没启动：检查容器内的进程是否存在`wechatListener.py`，不存在，请重启容器，或者手动执行

```json
nohup /usr/local/bin/python /xuexi/wechatListener.py >> /xuexi/user/wechat_listener.log 2>&1 &
```

#### 

#### 微信的使用

恭喜你完成所有配置，下面开始介绍功能的使用

配置成功之后，给公众号发送`/help`即可获得答复，如果没有，检查第一步中的 openid 是不是你的微信号。**只有主账号才能使用指令**

首先，给公众号发送 `/init` 初始化订阅号菜单,操作成功后等菜单出现，或者重新关注微信号，即可看到菜单

![image](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202207181105956.png)

点击`我的-账号编码`获取微信号编码。

发送`/add` 登录学 xi 账号，登录完成后获得 `数字ID_昵称`登录成功的消息。

> 可以利用网页版学习来获得数字ID，目前好像微信公众号无法直接获取数字ID
>
> 使用今日积分来获取登录跳转连接，通过默认浏览器跳转学习，可以获得数字ID

发送`/bind 微信编码 数字ID` 吧微信号和学 xi 账号绑定。如`/bind gw_djahdhfs 155555555`,不要有多余的空格。可以重复绑定，最后绑定的账号有效。

点`开始学xi`即可开始今天的学习。

`我的-今日积分` 获取今天学习积分。

### 其他账号绑定

让`用户A`先关注你的公众号。然后点击`账号编码`，并将编码给你。

给你的公众号发送`/add`指令，让`用户A`登录。你可以获得 ta 的数字 ID

给你的公众号发送`/bind 用户A的账号编码 用户A的数字ID`绑定成功后，`用户A`就可以自己学习和查分了。

`\unbind 微信编码` 解绑指定用户。

### 微信模板的使用

由于测试号的额度都有限制，所以将部分信息换方式发送，以降低某个额度用尽

在【模板消息接口】中添加两个模板。

![image](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202207181105534.png)

标题和内容任意，但是要保留`{{XXXX.DATA}}`。

登录模板

```json
{{name.DATA}} 需要登录， 如果页面不显示，请点击右上角，使用系统默认浏览器打开
```

今日得分模板

```json
{{score.DATA}}
```

将`模板ID`复制，并添加到配置文件中

```json
addition {
   wechat{
       appid = "第2步中获取的appid"
       appsecret = "第2步中获取的appsecret"
       openid = "第4步中获取的微信号"
       token = "随机字符串"
       login_tempid = "登录模板ID"
       score_tempid = "得分模板ID"
   }
}
```

**注意：** 复制 ID 的时候记得删除前后端的空格！！！删除空格！！！

登录模板配合`Scheme`环境变量 *移动端* 和 *Windows11* 可以直接拉起强国 APP。

