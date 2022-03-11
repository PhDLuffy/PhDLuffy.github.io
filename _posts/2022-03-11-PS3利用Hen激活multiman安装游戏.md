---
layout:     post
title:      PS3利用Hen激活multiman安装游戏
subtitle:   PS3运行游戏流程
date:       2022-03-11
author:     PhDLuffy
header-img: img/Blog/blog-20220-03-11-1.jpg
music-id: 445063
catalog: true
tags:
    - 软件



---



#! https://zhuanlan.zhihu.com/p/479439229

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# PS3利用Hen激活multiman安装游戏



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**

#### 光盘安装和dump光盘游戏

把PS3游戏光盘插入PS3

主界面打开hen激活破解

打开multiman，进入multiman环境

右划选择game选项，选择要复制的光盘游戏， 三角选择选项，选择copy，游戏从光盘下载到PS3内置硬盘HDD

![dump光盘文件](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203101646278.png)

#### FTP安装游戏文件夹

主界面打开hen激活破解

打开multiman，进入multiman环境，默认自动激活ftp服务器

打开transmit或其他ftp工具，ip为PS3的ip，端口号21，无登录账号和密码

![FPT成功连接PS3](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203101653576.png)

dev_hdd0→GAMES

这个文件夹是multiman环境dump游戏的文件夹，上传和下载游戏都通过这个文件夹。

dev_hdd0→PS3ISO

这个文件夹是放置PS3ISO格式游戏，各种插件通用位置

![GAMES文件夹](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203101657809.png)

#### FTP安装游戏ISO（不建议安装游戏文件夹格式，上传极易出错）

dev_hdd0→PS3ISO

![ISO文件夹](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203101738840.png)

#### PS3文件夹格式游戏转ISO格式游戏



正常流程

参考视频教程

[Using NTFS & exFAT USB Drives on webMAN MOD | PS1, PS2, PS3, PSP ISO Setup - YouTube](https://www.youtube.com/watch?v=iSt6VpxE2c8&list=PLSX4p5B08JrknsSY90VURvZAADc8rKwCp&index=8&t=931s)



1 下载webMAN MOD和 prepISO (NTFS/exFAT) [webMAN MOD v1.47.37 by aldostools - PS3 Brewology - PS3 PSP WII XBOX - Homebrew News, Saved Games, Downloads, and More!](https://store.brewology.com/ahomebrew.php?brewid=310)

![webMAN MOD](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203102325231.png)

2 下载PS3 ISO Tools  用于将文件夹格式游戏转化为ISO格式游戏



辅助插件的PKG文件通过FTP传输到PS3用于安装

FTP连接PS3，dev_hdd0→packages

PS3主页游戏→管理软件安装包PKG文件→安装PKG文件→PS3系统存储

X键安装



回到游戏

webMAN MOD  按住L1同时按X键安装完全版本的webMAN MOD

重启系统，HEN激活，打开游戏→我的游戏→webMAN设定，

在打开的网页设定风扇为自动模式（这一步可以减少风扇噪音）



U盘文件夹根目录下，新建PSXISO, PS2ISO, PS3ISO, PSPISO文件夹

对于PS1游戏，PS1游戏文件夹格式，文件夹内有BIN和CUE两个文件，外加一张封面图片，文件夹拷贝到PSXISO即可。

对于PS2和PS3游戏，拷贝游戏ISO文件到相应文件夹。（Hen激活下PS2玩光盘ISO格式游戏有问题，PS3完美）



#### PS3游戏文件夹格式转换为ISO格式

在Win系统下使用PS3 ISO Tools

PS3 system格式选择CFW，firmware选择4.76

点击Create ISO(s)

![PS3 ISO Tools](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203110002111.png)

去掉第一项，Patch Game(s) To Firmware

保留Rename ISO(s) To "[Game-Name]-[Game-ID]"

保留Use "makeps3iso" 很重要

点击 Continue

![PS3 ISO Tools设置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203110004694.png)

选择文件夹格式游戏文件夹，

选择ISO生成的位置，桌面即可。

然后等待生成结果

![运行过程](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203110007148.png)

![运行结果](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203110008650.png)



PS3插入U盘，点击PS3上面的Prepare exFAT/NTFS Drives

U盘的游戏会更新在webMAN Games里面，就是有webMAN 设定的那个位置下面的列表

游戏文件刷新需要重新点击Prepare exFAT、NTFS Drives（这个软件的运行相当于U盘文件刷新按钮）

![U盘游戏位置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202203110014004.png)

点击游戏ISO会进行虚拟光驱加载，回到PS3游戏界面，点击游戏运行即可。

相关文件小绿脸知乎同名ID，回复“PS3”即可。

以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~~~~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

