---
layout:     post
title:      Quest2死亡黑屏解决方法
subtitle:   一点也不省心的quest
date:       2022-04-14
author:     PhDLuffy
header-img: img/Blog/post-index-bg-dengziqi.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧



#! https://zhuanlan.zhihu.com/p/498398857

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# Quest2死亡黑屏解决方法

> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**

连接Oculus quest2到电脑

打开SideQuest

选择 Run ADB commands>Custom command



![sidequest运行ADB命令](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/image-20220414125646248.png)

运行以下命令

关闭距离传感器

`shell am broadcast -a com.oculus.vrpowermanager.prox_close`

打开距离传感器

`shell am broadcast -a com.oculus.vrpowermanager.automation_disable`

重启Quest2

#### 故障原因猜测

应该是头戴眼镜中间位置的传感器长时间运行会出错，

长时间处于持续检测状态造成传感器故障，

检测距离越来越短，

外网有小哥在上面贴上胶带可以解决

或者在佩戴时，如果出问题了，用手指深入眼镜，

挡在鼻子上方

瞬间就恢复正常，

以此证明确实是这个距离传感器出故障造成的系统黑屏。

![距离传感器位置](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/Inked%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20220414125837_LI(1649912693).jpg)

参考文献

[Proximity Sensor Issues - Oculus Community - 787353](https://forums.oculusvr.com/t5/Support/Proximity-Sensor-Issues/td-p/787353)



以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~~~~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

