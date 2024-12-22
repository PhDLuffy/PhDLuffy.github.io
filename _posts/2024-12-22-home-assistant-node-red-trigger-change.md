---
layout: post
title: Node-RED利用Trigger和Change来规避自循环问题
subtitle: 跳出无限循环的感应灯
date: 2024-12-22
author: PhDLuffy
header-img: img/Blog/17.jpg
music-id: 22854070
catalog: true
tags:
    - 日常

---

小米开源了米家和Home-assistant的联动，喜大普奔。

于是重新将所有的米家设备通过小米官方添加进了HA，但是小爱音箱pro竟然不能用自定义tts了，所以又重新用Miot单独把小米音箱Pro加进HA。

重新搞Node-RED的时候，发现之前没搞好的感应灯看论坛有解决办法。

目前的问题是，监控运动捕捉和灯的联动有相互循环的情况。

例如，检测到人，灯亮，然后等几分钟之后，灯灭。

灯灭的同时，由于监控画面亮度变化，误检测到有人，然后灯又亮了。

然后就无限循环起来。



解决方法：

利用Trigger，向flow流后面的Change发送值，比如先发射0，然后等待10s，再发送1

Change用于存储这个值为全局变量



然后在监控后方，加一个switch，默认向后传走整个流程的为`1`路，不向后走为0路



这样就可以达到忽略灯灭之后紧接着的那段时间的运动监测。



监测到有人，灯亮，灯延时5分钟，灯灭，灯灭的同时，Trigger传0给全局变量Change

与此同时，监控由于亮度变化，再次监测到有人，走到其后Switch跳到0不向后走流程

等待10s，Trigger又传1给全局变量Change，

等待下次监控监测到有人的时候，走到其后Switch跳到1走后面的全部流程。

以上。



![Node-RED流程图](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412222303045.png)
