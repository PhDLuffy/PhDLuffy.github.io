---
layout: post
title: 玩一玩空调
subtitle: Node-RED通过HomeAssistant操作红外空调
date: 2023-06-03
author: PhDLuffy
header-img: img/Blog/03.jpg
music-id: 1313104187
catalog: true
tags:
    - 奇技淫巧
---



### 空调接入系统

通过小爱音箱的红外学习功能，将红外设备-空调接入米家。

空调---小爱音箱Pro---米家---Miot---HomeAssistant---NodeRED



### NodeRED添加流程

接入NodeRED



![image-20230603141452956](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306031414027.png)



添加空调设备，设置时间，风速，温度等参数。



**时间**：使用inject添加启动时间



**温度**：使用Domain为Climate，Service为set_temperature，Enitity选择HA中显示的设备名，

Data为`{"temperature":24}`，温度数字不要加引号

![空调温度设定兼打开按键](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306031420376.png)

**风速**：使用Domain为select，Service为select_option，Enitity选择HA中显示的设备名，

Data为`{"option":"Fan Speed Up"}`，option选项为HA中的选项列表中的选项，此例为增大风速



![空调风速设定](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306031420377.png)

**关机**：使用Domain为climate，Service为turn_off，Enitity选择HA中显示的设备名

![关机设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306031425744.png)
