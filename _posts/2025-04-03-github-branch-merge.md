---
layout: post
title: Github新分支和分支合并
subtitle: 先分叉，搞坏了减掉，搞好了成为主支
date: 2025-04-03
author: PhDLuffy
header-img: img/header-img/4/3.jpg
music-id: 29802490
catalog: true
tags:
  - 日常
  - 软件
  - 博客
---

## Github Desktop

### 建立新分支

建立新的分支，在此分支上进行正常开发

> 记得Current Branch中选定新分支

![新建分支](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202504031338983.png)

### 正常编写代码

正常编写代码，正常comment

### 上传同步新分支

上传新分支会pull到新分支仓库

### 合并分支

同步新分支之后，新分支会提示Preview Pull Request，

点击即可提交请求给主分支，

只要之前不动主分支，只在新分支上继续开发，那么新分支的代码贡献上来就不可能会有冲突，

直接同意Merge即可，

网页端同意新分支的Merge pull request，这有点像别人给你贡献代码一样。

这样本地选择master主分支，可以进行正常Fetch origin，且有一个Merge pull request的comment出现在history里面

![Pull Request](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202504031346131.png)

### 删除开发分支

删除无用的开发分支。

确认处于主分支，`checkout`起到切换分支的作用, 后面接分支名

```bash
git checkout master
```

## ***\*本地删除分支\****

如果你还在一个分支上，那么 Git 是不允许你删除这个分支的。所以，请记得退出分支：`git checkout master`。

通过 `git branch -d <branch>`删除一个分支，比如：`git branch -d `分支名

当一个分支被推送并合并到远程分支后，`-d` 才会本地删除该分支。如果一个分支还没有被推送或者合并，那么可以使用`-D`强制删除它。

这就是本地删除分支的方法。

## ***\*远程删除分支\****

使用这个命令可以远程删除分支：`git push <remote> --delete <branch>`。

比如： `git push origin --delete `分支名，这个分支就被远程删除了。

> 记得开增强模式代理，vscode的终端需要代理才能连上github
