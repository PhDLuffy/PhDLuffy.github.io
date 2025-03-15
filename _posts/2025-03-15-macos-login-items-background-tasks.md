---
layout: post
title: 删除无效的MacOS登录项目
subtitle: 莫名其妙的卸载软件遗留，软件系统洁癖强迫症的福音
date: 2025-03-15
author: PhDLuffy
header-img: img/Blog/18.jpg
music-id: 5260494
catalog: true
tags:
  - 软件
---



![登录项目和后台](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202503152340849.png)

卸载软件之后，这个位置往往有莫名的残留，可以用以下方法删除

[如何删除「允许在后台」下面的app - Apple 社区](https://discussionschinese.apple.com/thread/254445461?sortBy=rank)

运行命令，输入密码，会在桌面生成一个`launch.txt`文件

```bash
sudo -- bash -c 'echo " - $(date) -"; while IFS= read -r eachPlist; do echo "-$eachPlist";  /usr/bin/defaults read "$eachPlist"; done <<< "$(/usr/bin/find /Library/LaunchDaemons /Library/LaunchAgents ~/Library/LaunchAgents /private/var/root/Library/LaunchAgents /private/var/root/Library/LaunchDaemons -name "*.plist")"; /usr/bin/defaults read com.apple.loginWindow LogoutHook; /usr/bin/defaults read com.apple.loginWindow LoginHook' > ~/Desktop/launch.txt
```

查看文件内容，

查看相关路径，

在对应文件夹路径下，删除对应文件名的`.plist`文件，重启即可。

![相关文件路径](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202503152343369.png)





