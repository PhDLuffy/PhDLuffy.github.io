---
layout:     post
title:      群晖下利用Docker将惠普P1106变身无线打印机并支持airprint？
subtitle:   老树开花
date:       2022-03-07
author:     PhDLuffy
header-img: img/Blog/blog-2022-03-07-01.jpg
music-id: 1464634416
catalog: true
tags:
    - 奇技淫巧
---



#! https://zhuanlan.zhihu.com/p/477312600

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# 群晖利用Docker将惠普P1106变身无线打印机并支持airprint



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**



#### 建立Docker容器

##### CUPS

打开群晖Docker，点击注册表，搜索cupsd，双击第一个olbat/cupsd下载

![建立Docker容器](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203071833406.jpg)

Docker文件夹下，新建airprint文件夹，此文件夹下，再新建avahi和config两个文件夹

![配置文件夹](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072225634.jpg)

通过termius或其他ssh软件进入群晖

下面的代码，逐行，复制，粘贴，回车

最后一行代码输入之后，会要求输入root的密码，输入密码然后回车即可。

> 注意大小写和空格

```json
sudo docker run -d --name=airprint \

--net="host" \

--privileged=true \

-e TZ="Asia/Shanghai" \

-e HOST_OS="Synology" \

-e "TCP_PORT_631"="631" \

-v "/volume1/docker/airprint/config":"/config" \

-v /dev:/dev \

-v "/volume1/docker/airprint/avahi":"/etc/avahi/services" \

-v /var/run/dbus:/var/run/dbus "olbat/cupsd"
```

命令成功运行完成会输出一长串数字

![命令行](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072226992.jpg)

##### 打印机

打开网页，输入网页管理网址，[群晖地址]:631

默认用户名和密码都是print

点击Administration，勾选右边的三个蓝色对号

点击Add Printer

![CUPS网页管理](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072228804.jpg)

如果之前操作没有问题，这里会跳出连接到群晖USB口上的打印机，如图，我的P1106打印机已经显示了，勾选进行下一步

![添加打印机](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072229722.jpg)

勾选上Share This Printer，下一步

![添加打印机](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072230064.jpg)

这一步非常关键，这个docker容器提供的HP驱动，带有requires proprietary plugin (en)的都不好使，选择下面的 Provide a PDD File，上传我提供的HP-ZJS的驱动才能完美运行，具体文件去小绿脸自取(ID和知乎相同)，关键字【惠普驱动】。

![打印机驱动](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072232907.jpg)

##### WIN,Mac,iPhone添加使用打印机

Windows，设置，打印机和扫描仪，添加打印机或扫描仪，自动会跳出打印机，点击添加即可，驱动会自动安装

![Win系统添加打印机](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203071856337.png)

Mac，设置，打印机与扫描仪。

点击左下方的小+号，跳出名称选择P1106，这里要注意，最下面的使用，系统会自动匹配我的那个HP-ZJS驱动，不要安装HP的官方驱动，点击添加。

![image-20220307185902824](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203071859894.png)

iPhone，随便找一个文件，点击分享，打印，打印机会自动跳转我们设置好的网络打印机。



![ios打印机选项](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072211670.jpg)

![ios系统打印机配置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203072136297.png)

以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

