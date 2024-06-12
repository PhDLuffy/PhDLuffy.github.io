---
layout: post
title: Zotero7的QuickLook失效了怎么办？
subtitle: 利用zotero-actions-tags的脚本拯救QuickLook插件
date: 2024-06-12
author: PhDLuffy
header-img: img/Blog/11.jpg
music-id: 2110380760
catalog: true
tags:
  - 软件
---

Zotero版本号: 7.0.0-beta.51+7c5600913

Actions and Tags for Zotero版本号: 1.0.0-beta.33

首选项→Actions & Tags→添加Actions

![设置界面](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202406122222774.png)

数据(脚本)：

```json
const ZoteroPane = require("ZoteroPane");
const Zotero_Tabs = require("Zotero_Tabs");
const document = require("document");

var lastItem = "";

async function getAttachmentPath(item) {
    if (item.isAttachment() && !item.isNote()) {
        return await item.getFilePathAsync();
    }
    else if (item.isRegularItem() && !item.isAttachment()) {
        let attachments = await item.getAttachments();
        for (let attachmentID of attachments) {
            let attachment = await Zotero.Items.getAsync(attachmentID);
			return await attachment.getFilePathAsync();
        }
    }
    return null;
}

async function openFile(item) {
    let filePath = await getAttachmentPath(item);
    if (!filePath) {
        return false;
    }
    // macbook path : -p /usr/bin/qlmanage
    // linux path : /usr/bin/sushi
    let applicationPath = "/usr/bin/qlmanage"
	let args = ['-p', filePath]
	Zotero.Utilities.Internal.exec(applicationPath, args);
}

async function oneKey(event) {
	var key = String.fromCharCode(event.which);
	let newItem = ZoteroPane.getSelectedItems()[0];
        if (Zotero_Tabs.selectedID === "zotero-pane") {
            if ((key == ' ' && !(event.ctrlKey || event.altKey || event.metaKey)) || (key == 'y' && event.metaKey && !(event.ctrlKey || event.altKey))) {
                if(newItem) {
                    lastItem = newItem;
                    return await openFile(newItem);
                }
                else {
                    return await openFile(lastItem);
                }
            }
        }
}

document.getElementById('zotero-items-tree').addEventListener("keydown", oneKey, false);
```

确定，给与数据权限，重启Zotero，

以上。
