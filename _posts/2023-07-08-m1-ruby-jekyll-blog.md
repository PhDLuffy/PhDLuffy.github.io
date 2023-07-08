---
layout: post
title: 第N次重复造轮子
subtitle: M1上安装arm版本jekyll本地调试静态blog
date: 2023-07-08
author: PhDLuffy
header-img: img/Blog/07.jpg
music-id: 224308
catalog: true
tags:
    - 软件

---



[The fastest and easiest way to install Ruby on a Mac in 2023](https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/#start-here-if-you-choose-the-long-and-manual-route)

## 准备工作

### 所要安装的软件

- brew
- Command Line Tools
- chruby ruby-install
- ruby gem
- bundler jekyll

### 软件作用

brew：用于安装chruby

Command Line Tools：安装brew必备

chruby：用于切换ruby版本

ruby-install：用于下载ruby

ruby：使用gem安装bundler和jekyll

bundler：用于安装jekyll插件

jekyll：用于静态网站本地调试

## 安装命令

[brew](https://brew.sh/)

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

> 安装过程中会提示安装mac的command-line-tools，确定安装即可

安装完成添加brew环境变量

```zsh
echo "eval $(/opt/homebrew/bin/brew shellenv)" >> ~/.zprofile
eval $(/opt/homebrew/bin/brew shellenv)
```

升级brew并检查brew是否正常

```zsh
brew update && brew doctor
```

>显示`Your system is ready to brew`为正常，可进行下一步

查看zsh面板是否是arm架构

```zsh
uname -m
```

> 显示`arm64`即为arm架构

安装chruby和ruby-install

```zsh
brew install chruby ruby-install
```

添加chruby环境变量

```zsh
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
```



以上安装完毕之后，

如果直接随意打开一个shell窗口输入`ruby --version`，显示的应该是mac系统自带ruby版本，`2.6.10`

我们尽量不去使用系统自带的ruby版本，

因为：

- 权限不高
- 无法管理切换版本

所以我们使用chruby来管理ruby



切换cd进入一个静态网站项目目录

在项目目录的根目录下新建文件.ruby-version，文件内容为`3.2.2`

> 这里的ruby版本为当前chruby安装ruby版本，可以自己指定

我们此时再cd到项目目录，然后`ruby --version`会发现，此时的ruby版本已经自动切换为chruby管理下的ruby3.2.2版本而不是系统的2.6.10版本



同时，在Gemfile中，在第一个gem之前，添加`ruby File.read(".ruby-version").strip`，

这样Gemfile也会去读取ruby的版本，chruby会自动切换对应的ruby版本



## 本地调试

首先，在Gemfile中，将`jekyll`的版本号定在4.3.0之上，也就是最低版本是4.3.1.

![image-20230708161718765](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307081617860.png)

删除项目目录中的Gemfile.lock文件

安装项目依赖

```zsh
bundle install 
```

![image-20230708161855204](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307081618236.png)

开启本地调试

```zsh
bundle exec jekyll s
```

![image-20230708161933540](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307081619333.png)

alt+鼠标左键点击Server address即可打开网页本地调试。
