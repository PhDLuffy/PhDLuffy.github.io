---
layout:     post
title:      Python处理JSON格式文件
subtitle:   懒人偷懒系列
date:       2021-09-24
author:     PhDLuffy
header-img: img/post-index-bg-dengziqi.jpg
music-id: 185700
catalog: true
tags:
    - 软件
---

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# Python处理JSON格式文件



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**



我上一个苹果手表应用的网站静态API的JSON文件要添加日语单词，

用Mac下的Smart JSON Editor并不方便，

因为日语平假名，罗马字和翻译分别属于三个不同的list，

我不仅要手动输入，还需要对准他们在数据数组中的位置，非常麻烦。



所以我利用Python来处理JSON文件，

一次输入自动定位位置进行填写，

这样又方便又准确。

大大的提升了效率。



#### 获取程序所在绝对位置

```python
import os

#### 获取当前文件路径

current_path = os.path.abspath(__file__)

fran_file_path = os.path.join(os.path.abspath(os.path.dirname(current_path)+os.path.sep),'fran.json')
```



#### 导入JSON文件

Python处理JSON文件需要json这个模块，`import json`即可

```open```后面跟json文件的路径地址

```json.load```用来读入json数据

```python
#### 导入JSON源文件

inputfile = open(fran_file_path)

test_riyu = json.load(inputfile)
```

#### 处理JSON数据

有两种选择，一种是在数组开头添加（可以引申为在数组任意位置添加，0表示在开头），另一种是在数组末尾位置添加。



建立三个输入变量，riyu, luomazi, fanyi

分别输入日语平假名，罗马字和翻译。

将数据添加入JSON文件数组开头的命令是

>0表示在数组0位置上，即首位。

```python
#### 编辑JSON

#### 在'我是list名称'这个list下面添加数组数据'数据名':数据值

riyu = input("请输入平假名")

luomazi = input("请输入罗马字")

fanyi = input("请输入翻译")

test_riyu['riyuData'].insert(0,{'riyu':riyu})

test_riyu['luomaziData'].insert(0,{'luomazi':luomazi})

test_riyu['fanyiData'].insert(0,{'fanyi':fanyi})
```

将数据添加入JSON文件数组末尾的命令是

JSON数据名['第一层list名称'].append({'第二层数组名称': 数据值})

```python
#### 编辑JSON

#### 在'我是list名称'这个list下面添加数组数据'数据名':数据值

riyu = input("请输入平假名")

luomazi = input("请输入罗马字")

fanyi = input("请输入翻译")

test_riyu['riyuData'].append({'riyu':riyu})

test_riyu['luomaziData'].append({'luomazi':luomazi})

test_riyu['fanyiData'].append({'fanyi':fanyi})
```

#### 导出JSON文件

```json.dumps```是导出数据到JSON文件

()的参数依次为：导出数据源名称，保证使用UTF-8编码，缩进4个空格，分隔符为逗号和冒号

```open```以及内部的参数```'w'```是新建一个可写的JSON文件

```write```将数据写入新建的JSON文件中

```close```关闭文件

```print("outputfile successed")```是输入一个提示，任务完成。

```python
#### 导出JSON文件

outputfile = json.dumps(test_riyu,ensure_ascii=False,indent=4,separators=(',', ':'))

outputjson = open(fran_file_path,'w')

outputjson.write(outputfile)

outputjson.close()

print("outputfile successed")
```

#### 完整代码如下


```python
import json
import os

#### 获取当前文件路径

current_path = os.path.abspath(__file__)

fran_file_path = os.path.join(os.path.abspath(os.path.dirname(current_path)+os.path.sep),'fran.json')

#### 导入JSON源文件

inputfile = open(fran_file_path)

test_riyu = json.load(inputfile)

#### 编辑JSON

#### 在'我是list名称'这个list下面添加数组数据'数据名':数据值

riyu = input("请输入平假名")

luomazi = input("请输入罗马字")

fanyi = input("请输入翻译")

#### 在数组末尾添加新元素

### test_riyu['riyuData'].append({'riyu':riyu})

### test_riyu['luomaziData'].append({'luomazi':luomazi})

### test_riyu['fanyiData'].append({'fanyi':fanyi})

#### 在数组开头添加新元素

test_riyu['riyuData'].insert(0,{'riyu':riyu})

test_riyu['luomaziData'].insert(0,{'luomazi':luomazi})

test_riyu['fanyiData'].insert(0,{'fanyi':fanyi})

#### 导出JSON文件

outputfile = json.dumps(test_riyu,ensure_ascii=False,indent=4,separators=(',', ':'))

outputjson = open(fran_file_path,'w')

outputjson.write(outputfile)

outputjson.close()

print("outputfile successed")


```

以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~~~~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

