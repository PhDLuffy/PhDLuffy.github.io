---
layout: post
title: 群晖DSM6.2.1-23824照片Moments智能相册bug修复
subtitle: 为了一碟醋包了一大锅饺子
date: 2024-12-26
author: PhDLuffy
header-img: img/Blog/05.jpg
music-id: 1480380024
catalog: true
tags:
    - 软件

---

为了给群晖DSM增加一块nvme硬盘，把整个工作流都打乱了，折腾了好几天。

## 群晖DSM的照片Moments有两个BUG

* 无法编解码封面(需要半洗白和更新ffmpeg)

* 无法AI人脸分析(替换AI库文件-libsynophoto-plugin-detection.so)

### 无法编解码封面

#### 半洗白

[群晖｜半洗白后moments正常显示人像、主题、预览「建议收藏」-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2080083)

chipgenius软件查询U盘的vid和pid，这一步在制作群晖DSM引导U盘的时候，应该记下来了。

![vid和pid](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262147362.png)

修改grub.cfg，ssh连接群晖DSM，

```bash
sudo -i  //获取root权限
mkdir -p /tmp/boot  //在/tmp目录下创建一个临时目录，名字随意，如：boot
cd /dev  //切换到dev目录
mount -t vfat synoboot1 /tmp/boot/  //将synoboot1分区挂载到boot
cd /tmp/boot/grub  //切换到grub目录
vim grub.cfg  //修改grub.cfg
```

将vid, pid填入下图红框中保存后退出，重启群晖

![grub修改](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262305581.png)

#### 更新ffmpeg

群晖自带的ffmpeg太老旧了，是2.7.1，我们安装新的ffmpeg替代系统自带的版本

订阅群晖第三方社区，套件-设置-添加-`http://packages.synocommunity.com`

![群晖添加第三方社群套件源显示无效的位置解决方案-图片2](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262320916.jpg)

[群晖添加第三方社群套件源显示无效的位置解决方案 | 若夜彼岸](https://www.ruoyer.com/dsm_crt.html)

> 需要更新下crt证书，否则会显示空白

```bash
sudo mv /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt.bak && sudo curl -Lko /etc/ssl/certs/ca-certificates.crt https://curl.se/ca/cacert.pem
```

在社区套件中安装`ffmpeg`，版本为4.3.2-38

![img](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262321306.png)

进入ssh，替换系统自身的ffmpeg，

```bash
sudo -i

mv /usr/bin/ffmpeg /usr/bin/ffmpeg_bak

cp /volume1/\@appstore/ffmpeg/bin/ffmpeg /usr/bin/

ffmpeg -version
```

以上动作，完成了半洗白和更新ffmpeg，此时即可解决照片缩略图不显示的问题。

### 无法AI人脸分析

[采坑无数，群晖NAS的相册备份最终解决方案！以及黑群晖6.2x版Moments智能相册补丁教程。_NAS存储_什么值得买](https://post.smzdm.com/p/a6dpg25o/)

[下载链接](https://pan.baidu.com/s/1Eq4GUuHgTLdyFEmZmaYwSQ)，提取码：2222

![采坑无数，群晖NAS的相册备份最终解决方案！以及黑群晖6.2x版Moments智能相册补丁教程。](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262328809.jpg)

WinSCP工具，root账号进入群晖根目录，

```bash
/var/packages/SynologyMoments/target/usr/lib/
```

将` libsynophoto-plugin-detection.so`原始文件添加`.bak`备份，上传` libsynophoto-plugin-detection.so`

修改文件属性，组和拥有者都选`SynologyMoments`，权限0755

![采坑无数，群晖NAS的相册备份最终解决方案！以及黑群晖6.2x版Moments智能相册补丁教程。](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202412262330862.jpg)

### Moments设置

进入Moments，

左下角头像-设置-相册-勾选自动创建的相册(人物，主题，位置，智能助手)

设置-常规-索引-全部重建索引

剩下的就是耐心的等待结果即可。

以上。



