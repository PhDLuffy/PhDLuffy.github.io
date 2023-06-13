---
layout: post
title: 终于可以在副屏上显示空气质量啦
subtitle: Macrodeck上显示HomeAssistant中三星空气监测数据
date: 2023-06-13
author: PhDLuffy
header-img: img/Blog/blog-20230612-01-mcrodeck.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧
---

## 准备工作

### 三星Smartthing外部访问Token

[Smartthing的Token获取地址](https://account.smartthings.com/tokens)

登录非大陆账号，美区或者港区都行

记录下生成的Token

## 连接HA

将三星空气监测接入HA，有两种方法

* 使用Home Assistant Cloud官方账号
* 使用自己的外部访问https

HA官方cloud账号收费，可免费试用一个月

自己的内网穿透需要添加https证书，过程比较繁琐

无论是HA官方还是自己的内网穿透，添加hppts证书之后，外链访问HA内部插件会出现部分访问不了的问题。



HA&rarr;配置&rarr;添加集成&rarr;SmartThings&rarr;添加Smartthing的Token

## 连接Macrodeck

### 电脑端Macrodeck添加HA中空气质量的变量

插件&rarr;HA插件&rarr;配置&rarr;变量

勾选air_quality的变量

![image-20230613114235006](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306131142147.png)

### 设置显示图标

空白图标右键编辑

点击标签右侧右上方斜箭头

变量空白处

```json
CO2     {sensor_air_quality_sensor_equivalent_carbon_dioxide_measurement}
CH2O    {format(sensor_air_quality_sensor_formaldehyde_measurement, "n:f2")}
TOVC    {format(sensor_air_quality_sensor_tvoc_measurement, "n:f3")}
PM2.5   {sensor_air_quality_sensor_fine_dust_level}
PM10    {sensor_air_quality_sensor_dust_level}
```

填入以上代码

第一列自定义名称

第二列为数据变量

"n:f2"代表取小数点后两位有效数字

![image-20230613114327794](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306131143898.png)

## 最终结果

![Macrodeck](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202306131139905.jpg)

