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

#### Anki软件插件设置

安装AnkiConnect插件，插件安装代码2055492159，插件网址为 [AnkiConnect](https://ankiweb.net/shared/info/2055492159)

在如图所在的AnkiConnect插件设置处填入如下代码

```json
{
    "apiKey": null,
    "apiLogPath": null,
    "ignoreOriginList": [],
    "webBindAddress": "127.0.0.1",
    "webBindPort": 8765,
    "webCorsOrigin": "http://localhost",
    "webCorsOriginList": [
        "http://localhost",
        "app://obsidian.md"
    ]
}
```



![image-20230920184638752](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202309201846786.png)

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

删除卡片单词

在<!--ID: XXXXXX-->前面添加DELETE，再点击同步按钮，即可同步删除Anki中的单词卡片

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