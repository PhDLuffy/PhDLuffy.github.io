---
layout: post
title: Mac安装arm版本的MATLAB
subtitle: 稳步升级旧设备和旧软件
date: 2025-07-22
author: PhDLuffy
header-img: img/header-img/7/22.jpg
music-id: 29802490
catalog: true
tags:
    - 软件

---

## matlab卸载旧版本

卸载matlab.app

```bash
/Applications/MATLAB_R20XXy.app
```

删除用户预设文件夹

**macOS：**

1. 打开Finder，Dock上的蓝色面孔

2. 在Finder窗口中, 点击 “Go” 菜单中点击 “Go to Folder”

3. 转到以下文件夹: ~/Library/Application Support/MathWorks/MATLAB

   > 删除对应年份版本的文件夹

   > 对于R2016a及更早版本：~/.matlab

## 安装matlab

验证软件压缩包和安装包哈希值，保证软件完整无误。

打开挂载的 dmg 文件并运行 InstallForMacOSAppleSilicon.app。如果您看到登录/密码/登录表单（安装程序可以访问互联网），则在“高级选项”中的右上角选择“设置模式”为“我有一个文件安装密钥”。在断网情况下,直接就可以进入这个选项，不需要手动选择。

> 需耐心等待软件安装界面自己弹出，需要较长时间，不要乱动

![56212332](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221523356.webp)

![123](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221523649.webp)

当您被要求“输入文件安装密钥”时，请输入以下密钥：

```bash
13524-34118-36356-04705-52908-49554-62658-50524-11699-58869-31128-29691-04297-03972-41841-25259-39095-20560-15057-30691-09676-24411-20994-63771-19270-40917
```

![36](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221524595.webp)

当您被要求“选择许可文件”时，浏览到/medicine/文件的文件夹中选择“license.lic”文件。

![2](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221524486.webp)

![54223](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221525744.gif)

然后选择您想要安装 Matlab 的文件夹。默认即可



选择安装组件,默认全选,自己根据需要选择也可以, 只选择`MATLAB`的安装大小为3.5G左右

![23](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221526501.webp)

![5](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221526052.webp)

![3434](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221526976.webp)

![65](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221526642.webp)

从包含/medicine/文件夹的文件夹中复制文件“libmwlmgrimpl.dylib”到

M芯片Mac路径`/Applications/MATLAB_R2024a.app/bin/maca64/matlab_startup_plugins/lmgrimpl/`

Intel芯片路径 `/Applications/MATLAB_R2024a.app/bin/maci64/matlab_startup_plugins/lmgrimpl/`

覆盖现有文件（是您在第4步中选择安装 Matlab 的位置）。如果您没有被要求覆盖，则您可能做错了什么（或 Matlab 安装不成功）！

![img](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221527802.gif)

#### 非常重要

把medicine文件夹中`license.lic`文件拷贝`/Applications/MATLAB_R2024a.app/licenses` (此步骤必须做!否则会提示许可证错误)

## 安装JAVA JRE[^2]

首先下载Arm版本所支持JRE，根据MathWorks官方提供的信息，需要下载并安装[Amazon Corretto11](https://corretto.aws/downloads/latest/amazon-corretto-11-aarch64-macos-jdk.pkg)，下载默认安装即可

安装好Corretto 11之后在根目录下执行如下的命令来使得Matlab找到对应的Java版本

```bash
/Applications/MATLAB_R2024a.app/bin/maca64/matlab_jenv /Library/Java/JavaVirtualMachines/amazon-corretto-11.jdk/Contents/Home
```
运行软件即可。

![56](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202507221529591.webp)


参考文献：

[^1]:[MathWorks MATLAB R2024a v24.1.0.2537033 Mac/Win/Linux官方原版下载+安装激活教程 完美破解版 - MAC萌新网](https://www.macxin.com/archives/23771.html#toc_4)

[^2]:[(3 封私信 / 2 条消息) Apple silicon 安装 Matlab Arm原生版本遇到的问题 - 知乎](https://zhuanlan.zhihu.com/p/672042028)
