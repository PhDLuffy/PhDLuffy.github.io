---
layout: post
title: Anki与Obsidian联动制卡
subtitle: iPhone上的anki卡片也能说话啦
date: 2023-03-01
author: PhDLuffy
header-img: img/Blog/03.jpg
music-id: 29802490
catalog: true
tags:
    - 学习

---

#### Obsidian插件中心Obsidian_to_Anki设置

Obsidian设置-第三方插件-Obsidian_to_Anki设置-Note type settings-Note Type Table

![插件中心](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202309201833598.png)

Basic这个卡片类型的Custom Regexp里面填上以下代码

`((?:[^\n][\n]?)+) #flashcard ?\n*((?:\n(?:^.{1,3}$|^.{4}(?<!<!--).*))+)`

![Obsidian_to_Anki设置](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202309201836001.png)

#### Obsidian库文件夹-anki文件夹-新建一个md文件

```markdown
TARGET DECK
Anki牌组名
```

> Anki牌组名会在Anki牌组库新建一个牌组

```markdown
单词 #flashcard
/音标/
翻译
```

单词后面要加一个空格，`#flashcard`是个标识位，

前面的内容是卡片正面，

后面的内容是卡片反面。

点击左边的Obsidian_to_Anki

> 记得打开anki应用

此时会Obsidian右边会有进度提示，success之后，

打开anki会看到单词卡片已经同步好了



![image-20230301010948125](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303010114693.png)





#### 添加音频

点击卡片组-单词卡片(可以多选)

点击Add Audio to Selected，会自动批量添加音频

![image-20230301011511457](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303010115546.png)

Source Field：选择Front

Destination：选择Front

单词和音标不要放在一起，否则音频会读两遍

![image-20230301012002643](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303010120776.png)



回到卡片组，复制音频链接，回到Obsidian，将音频连接复制到`#flashcard`前面，单词后面

![image-20230301011640381](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202303010116460.png)

点击Obsidian_to_Anki再同步一遍，将音频连接重新同步到Anki卡片中

> 如果不在Obsidian中重新添加一遍音频链接，同步会将Anki卡片中的音频链接覆盖删除掉
>
> 所以这个插件的同步是单向的，从Obsidian到Anki，以Obsidian为准

在Anki中同步上传到云上即可所有设备的Anki同步卡片且有音频。