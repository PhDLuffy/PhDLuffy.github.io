---
layout: post
title: 回忆往昔，好久没用邮件推送书到Kindle了
subtitle: 使用Calibre将自己书库的书通过邮件推送到Kindle
date: 2024-09-04
author: PhDLuffy
header-img: img/Blog/14.jpg
music-id: 2605806478
catalog: true
tags:
  - 软件
---

## 前记

心血来潮，登录亚马逊看了下，最近一次推送更新竟然都是2年前了，犹记得当时疯狂看书，推书，推漫画，可谓是把Kindle用到了极致。

后来Kindle退出了国内市场，我又用上了Calibre的本地推送管理，随大流把Kindle邮箱推送这一功能给抛弃了。

但是，我他喵的是美区账号啊，`.com`结尾的，跟国内`.cn`结尾的没半毛钱影响啊。

想到如此，甚是可笑了，所以重新拾起来，用邮箱推一推也更省事不是？

## 亚马逊Kindle官网设置

[Amazon.com： 管理您的內容和裝置](https://www.amazon.com/hz/mycd/myx#/home/settings/payment)

第一件事，先去亚马逊官网，Kindle的偏好设置，

【传输至Kindle的邮箱】，是Kindle接收书籍的邮箱，跟Kindle以及Kindle的APP一致，美区是`@kindle.com`结尾

> `@kindle.cn`结尾的是国区账号，已经被废掉了

【许可的个人文件电子邮箱】，是发送图书到Kindle的邮箱，是自己的邮箱或者其他网站的推送邮箱。

比如vol.moe漫画网站的推送邮箱等，这里相当于一个白名单，只有白名单里面的邮箱，亚马逊服务器才会接收邮箱文件。

## Calibre设置

[Calibre 使用教程之通过邮箱一键推送 Kindle 电子书 – 书伴](https://bookfere.com/post/11.html)

下载Calibre自不必多说

首选项-通过邮件分享

添加【传输至Kindle的邮箱】，也就是自己的Kindle邮箱账号，`@kindle.com`结尾的。

文件格式选择`EPUB`就行

发件人地址：发送图书的邮箱，自己的或者网站的

邮箱设置网页上有具体参数，QQ邮箱如图

### 测试发送邮件

全部设置完毕后点击右下角的“测试邮件发送”按钮，会出现“该操作会在屏幕上明文显示你的电子邮件地址密码。要继续吗？”的提示，点击“是”会弹出一个测试对话框，点击“测试”按钮，如果显示框出现“邮件已发出”即表示设置成功，否则请检查设置重试。点击“确定”按钮退出该对话框。点击左上角的“应用”按钮保存刚才的设置，结束设置。

\* 测试邮件发出后不久，亚马逊Kindle客服的自动回复系统会向你的推送邮箱发送一封主题为“你发送至Kindle的邮件未附任何文件附件”的提示邮件，请忽略。

![Calibre邮箱分享设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202409040108530.png)

### 一键推送图书

右键图书，连接/共享，右键发给`XXX@kindle.com`邮箱。

![image-20240904011136934](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202409040111962.png)

以上完成。
